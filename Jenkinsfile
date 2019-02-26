pipeline {
    agent any
    
stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
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
                       // sh "scp -i /home/jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                       echo 'Staging Deployment'
                    }
                }

                stage ("Deploy to Production"){
                    steps {
            //            sh "scp -i /home/jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                          echo "deploying to production"
                          timeout(time:5, unit:'DAYS'){
                            input message:'Approve Production Deployment'        
                          }
                          build job: 'Deploy-to-prod'                          
                    }
                    post{
                        success{
                            echo 'Code Deployed Successfully'
                        }

                        failure{
                            echo 'Deployment Failed'
                        }
                    }
                }
            }
        }
    }
}