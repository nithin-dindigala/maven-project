pipeline {
    agent any
    
    tools{
    
        maven 'localMaven'
    }
    
    parameters { 
         string(name: 'tomcat_dev', defaultValue: '3.86.114.120', description: 'Staging Server')
         
    } 
 
    triggers {
         pollSCM('* * * * *') // Polling Source Control
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
 
        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        bat "winscp -i "C:\\Users\\anu\\Desktop\\Cognizant\\Jenkins\\tomcat-demo.pem" **/target/*.war ec2-user@3.86.114.120:/var/lib/tomcat7/webapps"
                    }
                }
 
                
            }
        }
    }
}