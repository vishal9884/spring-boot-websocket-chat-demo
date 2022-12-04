pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "M3"
    }

    stages {
        
        stage('Preparation') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/callicoder/spring-boot-websocket-chat-demo.git'

            }
        }
        
        stage('Build') {
            steps {

                // To run Maven on a Windows agent, use
                bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }
            

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
        
        stage('Docker Build') {
    	
          steps {
          	bat 'docker build -t spring-boot-websocket-chat-demo .'
          }
        }
        
         
        stage('Deploy') {
    	
          steps {
            bat 'docker stop app_container'
            bat 'docker rm app_container'
          	bat 'docker run -d -p 5000:8080 --name app_container spring-boot-websocket-chat-demo'
          }
        }
    }
}
