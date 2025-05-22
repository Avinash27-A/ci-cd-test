pipeline {
    agent any

    environment {
        IMAGE_NAME = "ci-cd-test"
        CONTAINER_NAME = "ci-cd-test"
        APP_DIR = "ci-cd-test"
        GIT_REPO = "https://github.com/Avinash27-A/ci-cd-test.git"
        GIT_BRANCH = "main"
    }

    stages {

        stage('Prepare Directory') {
            steps {
                sh '''
                    rm -rf $APP_DIR
                    mkdir -p $APP_DIR
                '''
            }
        }

        stage('Clone Repository') {
            steps {
                dir("$APP_DIR") {
                    git branch: "${env.GIT_BRANCH}", credentialsId: 'Avinash6527', url: "${env.GIT_REPO}"
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                dir("$APP_DIR") {
                    sh 'docker build -t $IMAGE_NAME .'
                }
            }
        }

        stage('Reload and Restart systemd Service') {
            steps {
                sh '''
                    echo "Stopping old service (if running)..."
                    sudo systemctl stop $CONTAINER_NAME || true

                    echo "Reloading systemd daemon..."
                    sudo systemctl daemon-reload

                    echo "Starting service..."
                    sudo systemctl start $CONTAINER_NAME

                    echo "Service status:"
                    sudo systemctl status $CONTAINER_NAME --no-pager
                '''
            }
        }
    }

    post {
        failure {
            echo 'Pipeline failed.'
        }
        success {
            echo 'Deployment successful!'
        }
    }
}
