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
 }   
}
