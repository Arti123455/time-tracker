pipeline {
    agent any
     tools {
        maven 'Maven' 
        }
    stages {
        stage("Build"){
            steps{
                // mvn test
                bat 'mvn -DskipTests clean package'
                slackSend channel: 'springboot', message: 'Job started'
            }
            post {
                success {
                    echo "Now Archiving the Artifacts....."
                    archiveArtifacts artifacts: '**/*.war'
                }
            }   
        }
        stage("Test"){
            steps{
                bat 'mvn test'
                
            }
        }
        stage("Integration Test){
              steps{
                  bat 'mvn verify'
              }
          }
        stage("Deploy on Test"){
            steps{
                // deploy on container -> plugin
                deploy adapters: [tomcat8(credentialsId: 'credtomcat', path: '', url: 'http://54.159.195.182:8090')], contextPath: '/time-tracker', war: '**/*.war'
              
            }
            
        }
        
}
    post{
        always{
            echo "========always========"
        }
        success{
            echo "========pipeline executed successfully ========"
             slackSend channel: 'springboot', message: 'Job success'
        }
        failure{
            echo "========pipeline execution failed========"
             slackSend channel: 'springboot', message: 'Job failed'
        }
    }
}
