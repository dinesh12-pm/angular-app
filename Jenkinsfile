pipeline {
    agent any

    tools {
        nodejs "NodeJS-18"
    }

    environment {
        APP_NAME = "angular-app"
        IMAGE_NAME = "angular-app-image"
        CONTAINER_NAME = "angular-app-container"
    }

    stages {
        stage('Checkout') {
            steps {
                git(
                    branch: 'main',
                    credentialsId: 'github-creds',
                    url: 'https://github.com/dinesh12-pm/angular-app.git'
                )
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install -g @angular/cli'
                sh 'npm install'
            }
        }

        stage('Build Angular') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh """
                docker build -t ${IMAGE_NAME}:latest .
                """
            }
        }

        stage('Run Container') {
            steps {
                script {
                    // Stop old container if exists
                    sh """
                    if [ \$(docker ps -q -f name=${CONTAINER_NAME}) ]; then
                        docker stop ${CONTAINER_NAME} && docker rm ${CONTAINER_NAME}
                    fi
                    """
                    // Run new container
                    sh """
                    docker run -d --name ${CONTAINER_NAME} -p 80:80 ${IMAGE_NAME}:latest
                    """
                }
            }
        }
    }
}

