
pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS_ID = 'docker-cred'
        DOCKER_IMAGE = '777dockerjohn/my-fastapi-app'
        SONAR_SERVER_NAME = 'sonarqube-env'
        CONTAINER_NAME = 'fastapi-container'
    }

    stages {

        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'sonar-scanner'

                    withSonarQubeEnv("${SONAR_SERVER_NAME}") {
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }

        stage('Docker Build') {
            steps {
                sh '''
                    docker build -t ${DOCKER_IMAGE}:latest .
                '''
            }
        }

        stage('Docker Login and Push') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: "${DOCKER_CREDENTIALS_ID}",
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login \
                        -u "$DOCKER_USER" --password-stdin

                        docker push ${DOCKER_IMAGE}:latest

                        docker logout
                    '''
                }
            }
        }

        stage('Deploy Application') {
            steps {
                sh '''
                    docker stop ${CONTAINER_NAME} || true
                    docker rm ${CONTAINER_NAME} || true

                    docker run -d \
                    --name ${CONTAINER_NAME} \
                    -p 8000:8000 \
                    ${DOCKER_IMAGE}:latest
                '''
            }
        }
    }

    post {
        success {
            echo 'FastAPI application deployed successfully!'
        }

        failure {
            echo 'Pipeline failed. Check Jenkins console output.'
        }
    }
}
