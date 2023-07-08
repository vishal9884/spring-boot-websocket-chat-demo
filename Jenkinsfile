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
                git 'https://github.com/nicks204/spring-boot-websocket-chat-demo.git'
                
            }
        }
        
        stage('Build') {
            steps {

                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true clean package"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
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
        
        stage('Docker Image Build'){
            
            steps{
                
                //sh "docker rmi chatapplicationimage:$BUILD_NUMBER"
                sh "docker build -t chatapplicationimage:$BUILD_NUMBER ."
            }
        }
        
         stage('Docker Deploy'){
            
            steps{
                
                sh "docker stop chatappcontainer"
                sh "docker rm chatappcontainer"
                sh "docker run -d --name chatappcontainer -p 8088:8080 chatapplicationimage:$BUILD_NUMBER"
            }
        }
        
        stage('Test') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/nicks204/ChatApplicationTest.git'
                sh "mvn clean test"
                
            }
        }
    }
}
