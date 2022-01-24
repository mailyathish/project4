pipeline {
    agent any
	
	  tools
    {
       maven "Maven"
    }

    
    stages {
    stage('checkout') {
           steps {
             
                git branch: 'main', url: 'https://github.com/mailyathish/project4.git'
             
          }
        }
    
        stage('Build Application') { 
            steps {
                echo '=== Building Petclinic Application ==='
                sh 'mvn -B -DskipTests clean package' 
            }
        }
        
        stage('Test Application') {
            steps {
                echo '=== Testing Petclinic Application ==='
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
     stage('Docker Build and Tag') {
           steps {
              
               
               // sh '/Applications/Docker.app/Contents/Resources/bin/docker build -t rajuyathi/calculator:latest .' 
               sh 'docker build -t rajuyathi/petclinic-spinnaker-jenkins:latest .' 
                
               
          }
        }


    stage('Push Docker Image'){
	steps {
	
	withCredentials([string(credentialsId: 'DockerPWDS', variable: 'DockerPass')]) {
    	// some block

	//sh '/Applications/Docker.app/Contents/Resources/bin/docker login -u rajuyathi -p ${DockerPass}'
   sh 'docker login -u rajuyathi -p ${DockerPass}'

	
	 // some block
	//sh '/Applications/Docker.app/Contents/Resources/bin/docker push rajuyathi/calculator:latest'
   sh 'docker push rajuyathi/petclinic-spinnaker-jenkins:latest'
	}
   }
   }

stage('Run Container on Dev Server'){
	steps {
     	sshagent(['jenkins_docker']) {
             sh 'docker run -p 9090:9090 -d --name my-app rajuyathi/petclinic-spinnaker-jenkins'
       		//sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.35.179 docker rm -f my-app || true"
		    //sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.35.179 ${DOCKER_RUN}"
     	}

	}
	}
 }   
}
