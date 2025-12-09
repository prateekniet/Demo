pipeline {
    agent any

    environment {
        // --- CONFIGURATION ---
        DOCKER_USER = 'bhavyascaler' 
        IMAGE_TAG = "v${env.BUILD_NUMBER}"
        BASE_DIR = "voting-project/example-voting-app"
    }

    stages {
        // --- 1. GIT CHECKOUT (Explicit) ---
        stage('Checkout Code') {
            steps {
                script {
                    echo '--- Pulling Code from Git ---'
                    // Explicitly cloning the 'main' branch from your specific repo
                    git branch: 'main', 
                        url: 'https://github.com/scaler-bhavya/Demo-clone.git'
                }
            }
        }

        // --- 2. BUILD & PUSH ---
        stage('Build & Push Images') {
            parallel {
                stage('Vote App') {
                    steps {
                        script {
                            def image = "${DOCKER_USER}/vote:${IMAGE_TAG}"
                            echo "--- Building Vote App ---"
                            sh "docker build -t ${image} ./${BASE_DIR}/vote"
                            
                            withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                                sh "echo $PASS | docker login -u $USER --password-stdin"
                                sh "docker push ${image}"
                            }
                        }
                    }
                }

                stage('Result App') {
                    steps {
                        script {
                            def image = "${DOCKER_USER}/result:${IMAGE_TAG}"
                            echo "--- Building Result App ---"
                            sh "docker build -t ${image} ./${BASE_DIR}/result"
                            
                            withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                                sh "echo $PASS | docker login -u $USER --password-stdin"
                                sh "docker push ${image}"
                            }
                        }
                    }
                }

                stage('Worker App') {
                    steps {
                        script {
                            def image = "${DOCKER_USER}/worker:${IMAGE_TAG}"
                            echo "--- Building Worker App ---"
                            sh "docker build -t ${image} ./${BASE_DIR}/worker"
                            
                            withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                                sh "echo $PASS | docker login -u $USER --password-stdin"
                                sh "docker push ${image}"
                            }
                        }
                    }
                }
            }
        }

        // --- 3. DEPLOY TO KUBERNETES ---
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    echo "--- Applying Kubernetes Manifests ---"
                    
                    // Infrastructure
                    sh "kubectl apply -f ./${BASE_DIR}/k8s-specifications/db-deployment.yaml"
                    sh "kubectl apply -f ./${BASE_DIR}/k8s-specifications/db-service.yaml"
                    sh "kubectl apply -f ./${BASE_DIR}/k8s-specifications/redis-deployment.yaml"
                    sh "kubectl apply -f ./${BASE_DIR}/k8s-specifications/redis-service.yaml"

                    // Microservices
                    sh "kubectl apply -f ./${BASE_DIR}/k8s-specifications/vote-deployment.yaml"
                    sh "kubectl apply -f ./${BASE_DIR}/k8s-specifications/vote-service.yaml"
                    sh "kubectl apply -f ./${BASE_DIR}/k8s-specifications/result-deployment.yaml"
                    sh "kubectl apply -f ./${BASE_DIR}/k8s-specifications/result-service.yaml"
                    sh "kubectl apply -f ./${BASE_DIR}/k8s-specifications/worker-deployment.yaml"

                    echo "--- Updating Images to Version ${IMAGE_TAG} ---"
                    sh "kubectl set image deployment/vote vote=${DOCKER_USER}/vote:${IMAGE_TAG}"
                    sh "kubectl set image deployment/result result=${DOCKER_USER}/result:${IMAGE_TAG}"
                    sh "kubectl set image deployment/worker worker=${DOCKER_USER}/worker:${IMAGE_TAG}"
                    
                    sh "kubectl rollout status deployment/vote"
                }
            }
        }
    }

    post {
        always {
            sh "docker logout"
            sh "docker rmi ${DOCKER_USER}/vote:${IMAGE_TAG} ${DOCKER_USER}/result:${IMAGE_TAG} ${DOCKER_USER}/worker:${IMAGE_TAG} || true"
        }
    }
}
