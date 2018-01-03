pipeline {
    agent any
    
/*  parameters { 
         string(name: 'tomcat_dev', defaultValue: '35.166.210.154', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '34.209.233.6', description: 'Production Server')
    } */

    triggers {
         pollSCM('* * * * *') 
     }

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
                stage ('Static Analysis'){
                    steps {
                        //sh "scp -i /home/jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                        echo 'Static Analysis....'
                        build job: 'static-analysis'
                    }
                }

                stage ('Deploy to Staging'){
                    steps {
                        //sh "scp -i /home/jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                        echo 'Deploy to staging....'
                        build job: 'deploy-to-staging'
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        // sh "scp -i /home/jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                        echo 'Deploy to prod...'
                        build job: 'deploy-to-prod'
                    }
                }
            }
        }
    }
}
