// pipeline {
//   agent any

//   environment {
//     DOCKER_IMAGE = 'singhsajal/devopsified_reactapp' // replace with your DockerHub repo
//     DOCKER_CREDENTIALS_ID = 'dockerhub-singhsajal'      // Jenkins credential ID
//   }

//   stages {

//     stage('Checkout') {
//       steps {
//         echo "Cloning repository..."
//         checkout scm
//       }
//     }

//     stage('Build Docker Image') {
//       steps {
//         script {
//           echo "Building Docker image..."
//           docker.build("${DOCKER_IMAGE}:latest")
//         }
//       }
//     }

//     stage('Login to DockerHub') {
//       steps {
//         script {
//           echo "Logging in to DockerHub..."
//           docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS_ID}") {
//             echo "Logged in successfully."
//           }
//         }
//       }
//     }

//     stage('Push Docker Image') {
//       steps {
//         script {
//           docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS_ID}") {
//             docker.image("${DOCKER_IMAGE}:latest").push()
//           }
//         }
//       }
//     }

//   }

//   post {
//     success {
//       echo "Docker image pushed successfully!"
//     }
//     failure {
//       echo "Build or push failed!"
//     }
//   }
// }


pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'singhsajal/devopsified-reactapp'
        DOCKER_TAG = 'latest'
    }

    stages {

        stage('Checkout Code') {
            steps {
                echo '🔍 Cloning repository...'
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "🐳 Building Docker image ${DOCKER_IMAGE}:${DOCKER_TAG}"
                    sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
                }
            }
        }

        stage('Login to DockerHub') {
            steps {
                script {
                    echo "🔐 Logging into Docker Hub..."
                    withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh 'echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin'
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    echo "🚀 Pushing image to Docker Hub..."
                    sh "docker push ${DOCKER_IMAGE}:${DOCKER_TAG}"
                }
            }
        }

    }

    post {
        success {
            echo "✅ Build completed and image pushed: ${DOCKER_IMAGE}:${DOCKER_TAG}"
        }
        failure {
            echo "❌ Build failed. Check console output for errors."
        }
        always {
            echo "📋 Pipeline finished at: ${new Date()}"
        }
    }
}
