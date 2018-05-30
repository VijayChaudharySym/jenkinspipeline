pipeline {
    agent any
    
    parameters { 
         string(name: 'tomcat_dev', defaultValue: '192.168.2.100', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '192.168.2.100', description: 'Production Server')
    } 

    triggers {
         pollSCM('* * * * *') // Polling Source Control
     }

stages{
        stage('Build'){
            steps {
                sh '/usr/local/apache-maven-3.5.3/bin/mvn clean package'
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
                        //sh "scp -i /home/jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat/webapps"
                        sh "cp **/target/*.war /var/lib/tomcat/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        //sh "scp -i /home/jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat/webapps"
                        //sh "cp **/target/*.war /var/lib/tomcat/webapps"
                    }
                }
            }
        }
    }
}