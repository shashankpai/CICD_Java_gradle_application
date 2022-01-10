pipeline{
    agent{
        label "ubuntu_slave"
    }
    environment{
        VERSION = "${env.BUILD_ID}"
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
        stage("docker build and docker push") {
            steps{
                script{
                    withCredentials([string(credentialsId: 'docker_pass', variable: 'docker_password')]) {
                                sh '''
                                    docker build -t 127.0.0.1:8083/springapp:${VERSION} .
                                    docker login -u admin -p admin123 127.0.0.1:8083
                                    docker push 127.0.0.1:8083/springapp:${VERSION}
                                    docker rmi 127.0.0.1:8083/springapp:${VERSION}
                                '''
                    }
                    
                }
            }
        }
    }
}