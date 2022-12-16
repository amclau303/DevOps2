pipeline {
    agent any

    environment {
        registry = 'amclau303/project'
        registryCredential = 'dockerhub_id'
        dockerImage = ''
    }

    stages {
        stage('Build docker image') {
            steps {
                echo 'Building docker image'
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        stage('Push docker image to dockerhub') {
            steps {
                echo 'Pushing docker image'
                script {
                    docker.withRegistry('', registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }

            stage('Run docker container') {
                steps {
                    echo 'Running docker container :)'
                    sh 'docker run --rm --name cw2container -p 80:80 -d ' + registry + ":$BUILD_NUMBER"
                }
            }

            stage('Deply to kubernetes') {
                steps {
                script {
                    sh 'ssh ubuntu@54.224.167.126 kubectl set image deployments/kubernetes-server project=' + registry + ":$BUILD_NUMBER"
                }
                }
            }
    }
}
