pipeline{
    agent{
        label "ubuntu_slave"
    }
    stages{
        stage("sonar quality check"){
            agent {
                docker { 
                    image 'openjdk:11'
                }
            }
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                            sh 'chmod +x gradlew'
                            sh './gradlew sonarqube'
                    }
                }
            }
        }
    }
}