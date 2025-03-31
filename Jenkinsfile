pipeline {
    agent {
        docker {
            image 'sudipta244/myagent-nodejs:v2'
            args 'user root -v /var/run/docker.sock:/var/run/docker.sock'
        }
    }
    
   stages {
        stage('Checkout') {
            steps {
                    sh 'echo passed'
                    // git clone https://github.com/sudipta1/node-js-application
                    sh 'ls -lA'
            }
        }

        stage('Build and Push') {
             environment {
                        DOCKER_IMAGE = '$DOCKER_USERNAME/nodejs-application:$BUILD_NUMBER'
                        }
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
        
                        sh "docker build -t ${DOCKER_IMAGE} ."
                        sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin"
                        sh "docker push $DOCKER_IMAGE"
                    }
                }
            }
        }
   }