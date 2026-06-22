pipeline {
    agent { label 'agent-1' }

    stages {
        stage('Build') {
            steps {
                echo 'Compilation du projet...'
                sh 'chmod +x app.sh && ./app.sh'
            }
        }
        stage('Test') {
            steps {
                echo 'Exécution des tests...'
                sh 'chmod +x test.sh && ./test.sh'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Déploiement en cours...'
                echo 'Application déployée avec succès'
            }
        }
    }
}
