pipeline {
    agent {
        docker {
            image 'sudipta244/myagent-nodejs:v1'
            args '-v /var/run/docker.sock:/var/run/docker.sock -v /usr/local/bin/docker:/usr/local/bin/docker'
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
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
        
                        sh "docker build -t ${imageName} ."
                        sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin"
                        sh "docker push ${imageName}"
                    }
                }
            }
        }
   }