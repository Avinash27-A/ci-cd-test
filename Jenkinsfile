pipeline {
    agent any

    environment {
        IMAGE_NAME = "ci-cd-test"
        CONTAINER_NAME = "ci-cd-test"
        APP_DIR = "ci-cd-test"
        GIT_REPO = "https://github.com/Avinash27-A/ci-cd-test.git"
        GIT_BRANCH = "main"
        GIT_CREDENTIALS_ID = "Avinash6527"
    }

    stages {
        stage('Prepare Directory') {
            steps {
                sh '''
                    rm -rf "$APP_DIR"
                    mkdir -p "$APP_DIR"
                '''
            }
        }

        stage('Clone Repository') {
            steps {
                dir("$APP_DIR") {
                    git branch: "${GIT_BRANCH}", credentialsId: "${GIT_CREDENTIALS_ID}", url: "${GIT_REPO}"
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                dir("$APP_DIR") {
                    sh 'docker build -t "$IMAGE_NAME" .'
                }
            }
        }

        stage('Stop & Remove Existing Container') {
            steps {
                sh '''
                    docker stop "$CONTAINER_NAME" || true
                    docker rm "$CONTAINER_NAME" || true
                '''
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run -d -p 5000:5000 --name "$CONTAINER_NAME" "$IMAGE_NAME"'
            }
        }
    }
}
