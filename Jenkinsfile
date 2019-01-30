pipeline {
    agent any
    tools {
        maven 'localMaven'
    }
    stages{
        stage('Build'){
            steps {
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage('Deploying to stage'){
           steps{
              build job: 'deploy-to-stage'
           }
        
        }
        stage('Deploying to production'){
          steps{
              timeout(time:5,unit:'DAYS'){
              input message:'Approve Production deployment?'
              }
              build job: 'deploy-to-prod'
          }
          post{
            success{
               echo 'Code deployed to production'
            
            }
            failure{
            
               echo 'Deployment failed'
            }
          
          }
        
        }
    }
}