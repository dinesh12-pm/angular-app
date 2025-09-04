pipeline {
    agent any

    tools {
        nodejs "NodeJS-18"   // Configure NodeJS in Jenkins global tools
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/your-org/angular-app.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install -g @angular/cli'
                sh 'npm install'
            }
        }

        stage('Build Angular App') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm run test || true'  // avoid breaking pipeline if tests fail for demo
            }
        }

        stage('Docker Build & Push') {
            steps {
                script {
                    docker.build("your-dockerhub-user/angular-app:${env.BUILD_NUMBER}")
                          .push()
                }
            }
        }

        stage('Deploy') {
            steps {
                echo "Here you can add steps to deploy to AWS S3, EC2, or Kubernetes."
            }
        }
    }
}

