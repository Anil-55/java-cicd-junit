pipeline {
    agent any

    tools {
        maven 'Maven-3.8.5'
    }

    environment {
        IMAGE_NAME = 'my-java-app'
        IMAGE_TAG = "v1.${BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Anil-55/java-cicd-junit'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Run Unit Tests') {
            steps {
                script {
                    def result = sh(script: 'mvn test', returnStatus: true)
                    if (result == 0) {
                        echo '✅ Unit tests passed!'
                    } else {
                        error '❌ Unit tests failed!'
                    }
                }
            }
        }

        
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('MySonarQube') {
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t $IMAGE_NAME:$IMAGE_TAG .
                docker tag $IMAGE_NAME:$IMAGE_TAG $IMAGE_NAME:latest
                '''
            }
        }

        stage('Trivy Scan') {
            steps {
                sh '''
                trivy image $IMAGE_NAME:$IMAGE_TAG || true
                '''
            }
        }

        stage('Push Docker Image') {
            when {
                expression { return env.DOCKER_USERNAME != null }
            }
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh '''
                    echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
                    docker push $IMAGE_NAME:$IMAGE_TAG
                    docker push $IMAGE_NAME:latest
                    '''
                }
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed.'
        }
    }
}
