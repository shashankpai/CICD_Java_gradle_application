pipeline{
    agent{
        label "ubuntu_slave"
    }
    stages{
        stage("sonar quality check"){
            agent {
                docker { 
                    image 'openjdk:11'
                    args '-e SONAR_USER_HOME=/home/remote_user/Jenkins/.sonar'
                }
            }
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                            sh 'chmod +x gradlew'
                            sh './gradlew sonarqube --stacktrace --scan ' 
                    }
                }
            }
        }
    }
}