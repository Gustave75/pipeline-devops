pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "gustavedev/landing-page:latest"
        AWS_HOST = "13.48.148.134"
        AWS_USER = "ubuntu"
    }

    stages {
        stage('Build') {
            steps {
                echo 'Compilation du projet...'
                sh 'chmod +x app.sh && ./app.sh'
            }
        }
        stage('Test') {
            steps {
                echo 'Execution des tests...'
                sh 'chmod +x test.sh && ./test.sh'
            }
        }
        stage('Build Image Docker') {
            steps {
                echo 'Build de l image Docker...'
                sh 'docker build -t ${DOCKER_IMAGE} .'
            }
        }
        stage('Push DockerHub') {
            steps {
                echo 'Push sur DockerHub...'
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh 'docker push ${DOCKER_IMAGE}'
                }
            }
        }
        stage('Deploy sur AWS') {
            steps {
                echo 'Deploiement sur AWS...'
                sshagent(['aws-ssh-key']) {
                    sh '''
                        ssh -o StrictHostKeyChecking=no ${AWS_USER}@${AWS_HOST} \
                        "sudo docker pull ${DOCKER_IMAGE} && \
                         sudo docker stop landing-page || true && \
                         sudo docker rm landing-page || true && \
                         sudo docker run -d --name landing-page -p 80:80 ${DOCKER_IMAGE}"
                    '''
                }
            }
        }
    }
}
