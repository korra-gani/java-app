pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "ganesh1452/java-demo-app"
    }

    stages {

        stage('Clone') {
            steps {
                git 'https://github.com/korra-gani/java-app.git'
            }
        }

        stage('Build Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:latest .'
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    docker push $DOCKER_IMAGE:latest
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline success'
        }
        failure {
            echo 'Pipeline failed'
        }
    }
}
