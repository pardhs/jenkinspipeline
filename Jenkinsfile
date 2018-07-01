pipeline {
    agent any
    
    parameters { 
         string(name: 'tomcat_dev', defaultValue: '35.166.210.154', description: '192.168.1.21')
         string(name: 'tomcat_prod', defaultValue: '34.209.233.6', description: '192.168.1.19')
    } 

    triggers {
         pollSCM('* * * * *') // Polling Source Control
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
//sample
        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp -i /home/jenkins/tomcat-demo.pem **/target/*.war root@${params.tomcat_dev}:/home/tomcat/apache-tomcat-8.5.31/webapps/"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /home/jenkins/tomcat-demo.pem **/target/*.war root@${params.tomcat_prod}:/home/tomcat/apache-tomcat-8.5.31/webapps/"
                    }
                }
            }
        }
    }
}