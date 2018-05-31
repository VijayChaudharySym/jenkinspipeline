pipeline {
    agent any
    
    parameters { 
         string(name: 'tomcat_dev', defaultValue: '10.148.224.150', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '10.149.196.77', description: 'Production Server')
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
                        //sh "scp -i /root/.ssh/id_rsa **/target/*.war root@${params.tomcat_dev}:/var/lib/tomcat/webapps"
                        sh "scp -i /var/lib/jenkins/.ssh/id_rsa **/target/*.war root@${params.tomcat_dev}:/var/lib/tomcat/webapps"
                        //sh "cp **/target/*.war /var/lib/tomcat/webapps"
                        //sh "sudo -u jenkins cp **/target/*.war /var/lib/tomcat/webapps"

                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        //sh "scp -i /home/jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat/webapps"
                        //sh "scp -i /root/.ssh/id_rsa **/target/*.war root@${params.tomcat_dev}:/var/lib/tomcat/webapps"
                        sh "scp -i /var/lib/jenkins/.ssh/id_rsa **/target/*.war root@${params.tomcat_prod}:/var/lib/tomcat/webapps"
                        //sh "cp **/target/*.war /var/lib/tomcat/webapps"
                    }
                }
            }
        }
    }
}