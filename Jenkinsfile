pipeline {
    agent {
        docker {
            image 'sudipta244/myagent-nodejs:v2'
            args '--user root -v /var/run/docker.sock:/var/run/docker.sock'
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

            stage('Update Deployment File') {
                environment {
                    GIT_REPO = 'node-js-application'
                    GIT_USERNAME = 'sudipta1'
                }

                steps {
                    withCredentials([string(credentialsId: 'sudipta1', variable: 'GITHUB_TOKEN')]){

                        sh '''
                         git config --global --add safe.directory '*'
                         git config --global user.email "sudipta.nayak@nayak.com"
                         git config --global user.name "Sudipta Nayak"
                         BUILD_NUMBER=${BUILD_NUMBER}
                         sed -i "s/17/${BUILD_NUMBER}/g" kubernetes-manifests/deployment.yml
                         cat kubernetes-manifests/deployment.yml
                         git add kubernetes-manifests/deployment.yml
                         git commit -m "Updated deployment.yml into version ${BUILD_NUMBER}"
                         git push https://${GITHUB_TOKEN}@github.com/${GIT_USERNAME}/${GIT_REPO} HEAD:master
                            '''
                }
            }
        }
    }
}