pipeline {

    agent any

    environment {
        IMAGE_NAME = "docker-cicd-demo-rajath"
        CONTAINER_NAME = "docker-container"
    }

    stages {

        stage('Clone') {
            steps {
                echo 'Cloning source code...'

                git branch: 'main',
                    url: 'https://github.com/rajath-39/docker-cicd-demo-Rajath.git'
            }
        }

       stage('Test') {
    steps {
        echo 'Testing application...'
        sh '''
            echo "Current directory:"
            pwd

            echo "Files in workspace:"
            ls -la

            echo "Checking index.html:"
            test -f index.html

            echo "Checking Dockerfile:"
            test -f Dockerfile
        '''
    }
}

        stage('Docker Build') {
            steps {
                echo 'Building Docker image...'

                sh '''
                    docker build -t ${IMAGE_NAME}:latest .
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application...'

                sh '''
                    docker rm -f ${CONTAINER_NAME} 2>/dev/null || true

                    docker run -d \
                        --name ${CONTAINER_NAME} \
                        --restart unless-stopped \
                        -p 80:80 \
                        ${IMAGE_NAME}:latest
                '''
            }
        }
    }

    post {

        success {
            echo '================================='
            echo 'Deployment Successful!'
            echo '================================='
        }

        failure {
            echo '================================='
            echo 'Pipeline Failed!'
            echo '================================='
        }
    }
}