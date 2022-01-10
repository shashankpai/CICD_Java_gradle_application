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

                    timeout(time: 1, unit: 'HOURS') {
                      def qg = waitForQualityGate()
                      if (qg.status != 'OK') {
                           error "Pipeline aborted due to quality gate failure: ${qg.status}"
                      }
                    }
                }
            }
        }
    }
}