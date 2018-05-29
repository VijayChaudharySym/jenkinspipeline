pipeline {
    agent any
    stages{
        stage('Build'){
            steps {
                //sh 'mvn clean package'
                /usr/local/apache-maven-3.5.3/bin/mvn 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
    }
}