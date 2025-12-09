// // // // pipeline {
// // // //     agent any

// // // //     environment {
// // // //         // --- CONFIGURATION ---
// // // //         DOCKER_USER = 'bhavyascaler' 
// // // //         IMAGE_TAG = "v${env.BUILD_NUMBER}"
// // // //         BASE_DIR = "voting-project/example-voting-app"
// // // //     }

// // // //     stages {
// // // //         // --- 1. GIT CHECKOUT (Explicit) ---
// // // //         stage('Checkout Code') {
// // // //             steps {
// // // //                 script {
// // // //                     echo '--- Pulling Code from Git ---'
// // // //                     // Explicitly cloning the 'main' branch from your specific repo
// // // //                     git branch: 'main', 
// // // //                         url: 'https://github.com/scaler-bhavya/Demo-clone.git'
// // // //                 }
// // // //             }
// // // //         }

// // // //         // --- 2. BUILD & PUSH ---
// // // //         stage('Build & Push Images') {
// // // //             parallel {
// // // //                 stage('Vote App') {
// // // //                     steps {
// // // //                         script {
// // // //                             def image = "${DOCKER_USER}/vote:${IMAGE_TAG}"
// // // //                             echo "--- Building Vote App ---"
// // // //                             sh "docker build -t ${image} ./${BASE_DIR}/vote"
                            
// // // //                             withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
// // // //                                 sh "echo $PASS | docker login -u $USER --password-stdin"
// // // //                                 sh "docker push ${image}"
// // // //                             }
// // // //                         }
// // // //                     }
// // // //                 }

// // // //                 stage('Result App') {
// // // //                     steps {
// // // //                         script {
// // // //                             def image = "${DOCKER_USER}/result:${IMAGE_TAG}"
// // // //                             echo "--- Building Result App ---"
// // // //                             sh "docker build -t ${image} ./${BASE_DIR}/result"
                            
// // // //                             withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
// // // //                                 sh "echo $PASS | docker login -u $USER --password-stdin"
// // // //                                 sh "docker push ${image}"
// // // //                             }
// // // //                         }
// // // //                     }
// // // //                 }

// // // //                 stage('Worker App') {
// // // //                     steps {
// // // //                         script {
// // // //                             def image = "${DOCKER_USER}/worker:${IMAGE_TAG}"
// // // //                             echo "--- Building Worker App ---"
// // // //                             sh "docker build -t ${image} ./${BASE_DIR}/worker"
                            
// // // //                             withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
// // // //                                 sh "echo $PASS | docker login -u $USER --password-stdin"
// // // //                                 sh "docker push ${image}"
// // // //                             }
// // // //                         }
// // // //                     }
// // // //                 }
// // // //             }
// // // //         }

// // // //         // --- 3. DEPLOY TO KUBERNETES ---
// // // //         stage('Deploy to Kubernetes') {
// // // //             steps {
// // // //                 script {
// // // //                     echo "--- Applying Kubernetes Manifests ---"
                    
// // // //                     // Infrastructure
// // // //                     sh "kubectl apply -f ./${BASE_DIR}/k8s-specifications/db-deployment.yaml"
// // // //                     sh "kubectl apply -f ./${BASE_DIR}/k8s-specifications/db-service.yaml"
// // // //                     sh "kubectl apply -f ./${BASE_DIR}/k8s-specifications/redis-deployment.yaml"
// // // //                     sh "kubectl apply -f ./${BASE_DIR}/k8s-specifications/redis-service.yaml"

// // // //                     // Microservices
// // // //                     sh "kubectl apply -f ./${BASE_DIR}/k8s-specifications/vote-deployment.yaml"
// // // //                     sh "kubectl apply -f ./${BASE_DIR}/k8s-specifications/vote-service.yaml"
// // // //                     sh "kubectl apply -f ./${BASE_DIR}/k8s-specifications/result-deployment.yaml"
// // // //                     sh "kubectl apply -f ./${BASE_DIR}/k8s-specifications/result-service.yaml"
// // // //                     sh "kubectl apply -f ./${BASE_DIR}/k8s-specifications/worker-deployment.yaml"

// // // //                     echo "--- Updating Images to Version ${IMAGE_TAG} ---"
// // // //                     sh "kubectl set image deployment/vote vote=${DOCKER_USER}/vote:${IMAGE_TAG}"
// // // //                     sh "kubectl set image deployment/result result=${DOCKER_USER}/result:${IMAGE_TAG}"
// // // //                     sh "kubectl set image deployment/worker worker=${DOCKER_USER}/worker:${IMAGE_TAG}"
                    
// // // //                     sh "kubectl rollout status deployment/vote"
// // // //                 }
// // // //             }
// // // //         }
// // // //     }

// // // //     post {
// // // //         always {
// // // //             sh "docker logout"
// // // //             sh "docker rmi ${DOCKER_USER}/vote:${IMAGE_TAG} ${DOCKER_USER}/result:${IMAGE_TAG} ${DOCKER_USER}/worker:${IMAGE_TAG} || true"
// // // //         }
// // // //     }
// // // // }

// // // pipeline {
// // //     agent any
// // //     environment {
// // //         DOCKER_USER = 'bhavyascaler'
// // //         IMAGE_TAG = "v${env.BUILD_NUMBER}"
// // //         BASE_DIR = "voting-project/example-voting-app"
// // //         // Ensure this matches where your deployment.yaml is located
// // //         K8S_DIR = "k8s-specifications" 
// // //         GIT_REPO_URL = "github.com/scaler-bhavya/Demo-clone.git"
// // //         GIT_BRANCH = "main"
// // //     }
// // //     stages {
// // //         stage('Checkout') {
// // //             steps { git branch: "${GIT_BRANCH}", url: "https://${GIT_REPO_URL}" }
// // //         }
// // //         stage('Build & Push') {
// // //             parallel {
// // //                 stage('Vote') {
// // //                     steps {
// // //                         script {
// // //                             def image = "${DOCKER_USER}/vote:${IMAGE_TAG}"
// // //                             sh "docker build -t ${image} ./${BASE_DIR}/vote"
// // //                             withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
// // //                                 sh "echo $PASS | docker login -u $USER --password-stdin"
// // //                                 sh "docker push ${image}"
// // //                             }
// // //                         }
// // //                     }
// // //                     stage('Result') {
// // //                         steps {
// // //                             script {
// // //                                 def image = "${DOCKER_USER}/result:${IMAGE_TAG}"
// // //                                 sh "docker build -t ${image} ./${BASE_DIR}/result"
// // //                                 withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
// // //                                     sh "docker push ${image}"
// // //                                 }
// // //                             }
// // //                         }
// // //                     }
// // //                     stage('Worker') {
// // //                         steps {
// // //                             script {
// // //                                 def image = "${DOCKER_USER}/worker:${IMAGE_TAG}"
// // //                                 sh "docker build -t ${image} ./${BASE_DIR}/worker"
// // //                                 withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
// // //                                     sh "docker push ${image}"
// // //                                 }
// // //                             }
// // //                         }
// // //                     }
// // //                 }
// // //             }
// // //         }
// // //         stage('GitOps Update') {
// // //             steps {
// // //                 script {
// // //                     withCredentials([usernamePassword(credentialsId: 'github-credentials', passwordVariable: 'GIT_PASS', usernameVariable: 'GIT_USER')]) {
// // //                         sh """
// // //                             git config user.email "jenkins-bot@example.com"
// // //                             git config user.name "Jenkins Pipeline"
                            
// // //                             # Update image tags in YAML files
// // //                             sed -i "s|image: ${DOCKER_USER}/vote:.*|image: ${DOCKER_USER}/vote:${IMAGE_TAG}|g" ${K8S_DIR}/*.yaml
// // //                             sed -i "s|image: ${DOCKER_USER}/result:.*|image: ${DOCKER_USER}/result:${IMAGE_TAG}|g" ${K8S_DIR}/*.yaml
// // //                             sed -i "s|image: ${DOCKER_USER}/worker:.*|image: ${DOCKER_USER}/worker:${IMAGE_TAG}|g" ${K8S_DIR}/*.yaml
                            
// // //                             # Push changes
// // //                             if git status | grep -q "modified"; then
// // //                                 git add ${K8S_DIR}/*.yaml
// // //                                 git commit -m "CI: Update images to ${IMAGE_TAG}"
// // //                                 git push https://${GIT_USER}:${GIT_PASS}@${GIT_REPO_URL} HEAD:${GIT_BRANCH}
// // //                             fi
// // //                         """
// // //                     }
// // //                 }
// // //             }
// // //         }
// // //     }
// // // }


// // pipeline {
// //     agent any
// //     environment {
// //         DOCKER_USER = 'bhavyascaler'
// //         IMAGE_TAG = "v${env.BUILD_NUMBER}"
// //         BASE_DIR = "voting-project/example-voting-app"
// //         // Ensure this matches where your deployment.yaml is located
// //         K8S_DIR = "k8s-specifications" 
// //         GIT_REPO_URL = "github.com/scaler-bhavya/Demo-clone.git"
// //         GIT_BRANCH = "main"
// //     }
// //     stages {
// //         stage('Checkout') {
// //             steps { git branch: "${GIT_BRANCH}", url: "https://${GIT_REPO_URL}" }
// //         }
// //         stage('Build & Push') {
// //             parallel {
// //                 stage('Vote') {
// //                     steps {
// //                         script {
// //                             def image = "${DOCKER_USER}/vote:${IMAGE_TAG}"
// //                             sh "docker build -t ${image} ./${BASE_DIR}/vote"
// //                             withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
// //                                 sh "echo $PASS | docker login -u $USER --password-stdin"
// //                                 sh "docker push ${image}"
// //                             }
// //                         }
// //                     }
// //                 }
// //                 stage('Result') {
// //                     steps {
// //                         script {
// //                             def image = "${DOCKER_USER}/result:${IMAGE_TAG}"
// //                             sh "docker build -t ${image} ./${BASE_DIR}/result"
// //                             withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
// //                                 sh "docker push ${image}"
// //                             }
// //                         }
// //                     }
// //                 }
// //                 stage('Worker') {
// //                     steps {
// //                         script {
// //                             def image = "${DOCKER_USER}/worker:${IMAGE_TAG}"
// //                             sh "docker build -t ${image} ./${BASE_DIR}/worker"
// //                             withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
// //                                 sh "docker push ${image}"
// //                             }
// //                         }
// //                     }
// //                 }
// //             }
// //         }
// //         stage('GitOps Update') {
// //             steps {
// //                 script {
// //                     withCredentials([usernamePassword(credentialsId: 'github-credentials', passwordVariable: 'GIT_PASS', usernameVariable: 'GIT_USER')]) {
// //                         sh """
// //                         git config user.email "jenkins-bot@example.com"
// //                         git config user.name "Jenkins Pipeline"
                        
// //                         # Update image tags in YAML files
// //                         sed -i "s|image: ${DOCKER_USER}/vote:.*|image: ${DOCKER_USER}/vote:${IMAGE_TAG}|g" ${K8S_DIR}/*.yaml
// //                         sed -i "s|image: ${DOCKER_USER}/result:.*|image: ${DOCKER_USER}/result:${IMAGE_TAG}|g" ${K8S_DIR}/*.yaml
// //                         sed -i "s|image: ${DOCKER_USER}/worker:.*|image: ${DOCKER_USER}/worker:${IMAGE_TAG}|g" ${K8S_DIR}/*.yaml
                        
// //                         # Push changes
// //                         if git status | grep -q "modified"; then
// //                           git add ${K8S_DIR}/*.yaml
// //                           git commit -m "CI: Update images to ${IMAGE_TAG}"
// //                           git push https://${GIT_USER}:${GIT_PASS}@${GIT_REPO_URL} HEAD:${GIT_BRANCH}
// //                         fi
// //                         """
// //                     }
// //                 }
// //             }
// //         }
// //     }
// // }


// pipeline {
//     agent any
//     environment {
//         DOCKER_USER = 'bhavyascaler'
//         IMAGE_TAG = "v${env.BUILD_NUMBER}"
        
//         // Path to source code
//         BASE_DIR = "voting-project/example-voting-app"
        
//         // FIXED: Full path to your K8s YAMLs
//         K8S_DIR = "voting-project/example-voting-app/k8s-specifications"
        
//         GIT_REPO_URL = "github.com/scaler-bhavya/Demo-clone.git"
//         GIT_BRANCH = "main"
//     }
//     stages {
//         stage('Checkout') {
//             steps { git branch: "${GIT_BRANCH}", url: "https://${GIT_REPO_URL}" }
//         }
//         stage('Build & Push') {
//             parallel {
//                 stage('Vote') {
//                     steps {
//                         script {
//                             def image = "${DOCKER_USER}/vote:${IMAGE_TAG}"
//                             sh "docker build -t ${image} ./${BASE_DIR}/vote"
//                             withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
//                                 sh "echo $PASS | docker login -u $USER --password-stdin"
//                                 sh "docker push ${image}"
//                             }
//                         }
//                     }
//                 }
//                 stage('Result') {
//                     steps {
//                         script {
//                             def image = "${DOCKER_USER}/result:${IMAGE_TAG}"
//                             sh "docker build -t ${image} ./${BASE_DIR}/result"
//                             withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
//                                 sh "docker push ${image}"
//                             }
//                         }
//                     }
//                 }
//                 stage('Worker') {
//                     steps {
//                         script {
//                             def image = "${DOCKER_USER}/worker:${IMAGE_TAG}"
//                             sh "docker build -t ${image} ./${BASE_DIR}/worker"
//                             withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
//                                 sh "docker push ${image}"
//                             }
//                         }
//                     }
//                 }
//             }
//         }
//         stage('GitOps Update') {
//             steps {
//                 script {
//                     withCredentials([usernamePassword(credentialsId: 'github-credentials', passwordVariable: 'GIT_PASS', usernameVariable: 'GIT_USER')]) {
//                         sh """
//                         git config user.email "jenkins-bot@example.com"
//                         git config user.name "Jenkins Pipeline"

//                         # Update image tags in YAML files
//                         sed -i "s|image: ${DOCKER_USER}/vote:.*|image: ${DOCKER_USER}/vote:${IMAGE_TAG}|g" ${K8S_DIR}/*.yaml
//                         sed -i "s|image: ${DOCKER_USER}/result:.*|image: ${DOCKER_USER}/result:${IMAGE_TAG}|g" ${K8S_DIR}/*.yaml
//                         sed -i "s|image: ${DOCKER_USER}/worker:.*|image: ${DOCKER_USER}/worker:${IMAGE_TAG}|g" ${K8S_DIR}/*.yaml
                        
//                         # Push changes
//                         if git status | grep -q "modified"; then
//                           git add ${K8S_DIR}/*.yaml
//                           git commit -m "CI: Update images to ${IMAGE_TAG}"
//                           # We escape variables here to avoid Groovy script security warnings
//                           git push https://\${GIT_USER}:\${GIT_PASS}@${GIT_REPO_URL} HEAD:${GIT_BRANCH}
//                         else
//                           echo "No changes to commit."
//                         fi
//                         """
//                     }
//                 }
//             }
//         }
//     }
// }



// pipeline {
//     agent any
//     environment {
//         DOCKER_USER = 'bhavyascaler'
//         IMAGE_TAG = "v${env.BUILD_NUMBER}"
        
//         // Path to source code
//         BASE_DIR = "voting-project/example-voting-app"
        
//         // Full path to your K8s YAMLs
//         K8S_DIR = "voting-project/example-voting-app/k8s-specifications"
        
//         GIT_REPO_URL = "github.com/scaler-bhavya/Demo-clone.git"
//         GIT_BRANCH = "main"
//     }
//     stages {
//         stage('Checkout') {
//             steps { git branch: "${GIT_BRANCH}", url: "https://${GIT_REPO_URL}" }
//         }
//         stage('Build & Push') {
//             parallel {
//                 stage('Vote') {
//                     steps {
//                         script {
//                             def image = "${DOCKER_USER}/vote:${IMAGE_TAG}"
//                             sh "docker build -t ${image} ./${BASE_DIR}/vote"
//                             withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
//                                 sh "echo $PASS | docker login -u $USER --password-stdin"
//                                 sh "docker push ${image}"
//                             }
//                         }
//                     }
//                 }
//                 stage('Result') {
//                     steps {
//                         script {
//                             def image = "${DOCKER_USER}/result:${IMAGE_TAG}"
//                             sh "docker build -t ${image} ./${BASE_DIR}/result"
//                             withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
//                                 // ADDED LOGIN HERE
//                                 sh "echo $PASS | docker login -u $USER --password-stdin"
//                                 sh "docker push ${image}"
//                             }
//                         }
//                     }
//                 }
//                 stage('Worker') {
//                     steps {
//                         script {
//                             def image = "${DOCKER_USER}/worker:${IMAGE_TAG}"
//                             sh "docker build -t ${image} ./${BASE_DIR}/worker"
//                             withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
//                                 // ADDED LOGIN HERE
//                                 sh "echo $PASS | docker login -u $USER --password-stdin"
//                                 sh "docker push ${image}"
//                             }
//                         }
//                     }
//                 }
//             }
//         }
//         stage('GitOps Update') {
//             steps {
//                 script {
//                     withCredentials([usernamePassword(credentialsId: 'github-credentials', passwordVariable: 'GIT_PASS', usernameVariable: 'GIT_USER')]) {
//                         sh """
//                         git config user.email "jenkins-bot@example.com"
//                         git config user.name "Jenkins Pipeline"

//                         # Update image tags in YAML files
//                         sed -i "s|image: ${DOCKER_USER}/vote:.*|image: ${DOCKER_USER}/vote:${IMAGE_TAG}|g" ${K8S_DIR}/*.yaml
//                         sed -i "s|image: ${DOCKER_USER}/result:.*|image: ${DOCKER_USER}/result:${IMAGE_TAG}|g" ${K8S_DIR}/*.yaml
//                         sed -i "s|image: ${DOCKER_USER}/worker:.*|image: ${DOCKER_USER}/worker:${IMAGE_TAG}|g" ${K8S_DIR}/*.yaml
                        
//                         # Push changes
//                         if git status | grep -q "modified"; then
//                           git add ${K8S_DIR}/*.yaml
//                           git commit -m "CI: Update images to ${IMAGE_TAG}"
//                           git push https://\${GIT_USER}:\${GIT_PASS}@${GIT_REPO_URL} HEAD:${GIT_BRANCH}
//                         else
//                           echo "No changes to commit."
//                         fi
//                         """
//                     }
//                 }
//             }
//         }
//     }
// }



pipeline {
    agent any
    environment {
        // --- CONFIGURATION ---
        DOCKER_USER = 'bhavyascaler'
        IMAGE_TAG = "v${env.BUILD_NUMBER}"
        
        // Path to source code (Where the Dockerfiles are)
        BASE_DIR = "voting-project/example-voting-app"
        
        // Full path to your K8s YAMLs (Where the Deployment files are)
        K8S_DIR = "voting-project/example-voting-app/k8s-specifications"
        
        GIT_REPO_URL = "github.com/scaler-bhavya/Demo-clone.git"
        GIT_BRANCH = "main"
    }
    stages {
        stage('Checkout') {
            steps { 
                // Pulls your code from GitHub
                git branch: "${GIT_BRANCH}", url: "https://${GIT_REPO_URL}" 
            }
        }
        stage('Build & Push') {
            parallel {
                stage('Vote App') {
                    steps {
                        script {
                            def image = "${DOCKER_USER}/vote:${IMAGE_TAG}"
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
                            sh "docker build -t ${image} ./${BASE_DIR}/result"
                            withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                                // FIXED: Added Login here
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
                            sh "docker build -t ${image} ./${BASE_DIR}/worker"
                            withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                                // FIXED: Added Login here
                                sh "echo $PASS | docker login -u $USER --password-stdin"
                                sh "docker push ${image}"
                            }
                        }
                    }
                }
            }
        }
        stage('GitOps Update') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'github-credentials', passwordVariable: 'GIT_PASS', usernameVariable: 'GIT_USER')]) {
                        sh """
                        git config user.email "jenkins-bot@example.com"
                        git config user.name "Jenkins Pipeline"

                        # Update image tags in YAML files
                        # Note: This looks for 'image: bhavyascaler/vote:...' in your YAMLs
                        sed -i "s|image: ${DOCKER_USER}/vote:.*|image: ${DOCKER_USER}/vote:${IMAGE_TAG}|g" ${K8S_DIR}/*.yaml
                        sed -i "s|image: ${DOCKER_USER}/result:.*|image: ${DOCKER_USER}/result:${IMAGE_TAG}|g" ${K8S_DIR}/*.yaml
                        sed -i "s|image: ${DOCKER_USER}/worker:.*|image: ${DOCKER_USER}/worker:${IMAGE_TAG}|g" ${K8S_DIR}/*.yaml
                        
                        # Check if anything changed
                        if git status | grep -q "modified"; then
                          git add ${K8S_DIR}/*.yaml
                          git commit -m "CI: Update images to ${IMAGE_TAG}"
                          # Authenticate and Push
                          git push https://\${GIT_USER}:\${GIT_PASS}@${GIT_REPO_URL} HEAD:${GIT_BRANCH}
                        else
                          echo "No changes needed (or sed didn't find the image tag in YAMLs)"
                        fi
                        """
                    }
                }
            }
        }
    }
}
