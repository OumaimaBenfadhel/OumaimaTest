pipeline {
    agent any
    environment {
        IMAGE_TAG = "latest"
        DOCKERHUB_USERNAME =  "${env.DOCKERHUB_USERNAME}"
        }
    stages {
        stage('Fetch Source Code') {
			steps {
				script {
					try {
                        echo "Cleaning Workspace"
						sh 'git status'
                        
					} catch (Exception e) {
						error "Fail in Fetch Source Code stage: ${e.message}"
					}
				}
			}
			}

        stage('Build docker image') {
                steps {
                    echo "Building docker image"
                    sh 'docker build -t $DOCKERHUB_USERNAME/getting-started:$IMAGE_TAG .'
                        }
                    }

        stage('Push images to Dockerhub') {
                steps{
                        script{
				withCredentials([string(credentialsId: 'dockerhub_cred', variable:'dockerhub_cred')]) {
		                        sh 'echo ${dockerhub_cred} | docker login -u $DOCKERHUB_USERNAME --password-stdin'
		                        sh 'docker push $DOCKERHUB_USERNAME/getting-started:$IMAGE_TAG'
				}
                        }
                    }
                }
	stage('Deploy') {
                steps{
                        script{
				sh 'docker compose down'
                        	sh 'docker compose up -d --build'
                        }
                    }
                }
        
    }
    
    }
