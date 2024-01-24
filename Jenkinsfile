pipeline {
    agent any
    stages {
        stage("Verify tooling") {
            steps {
                sh '''
                    docker info
                    docker version
                    docker compose version
                '''
            }
        }
        stage("Clear all running docker containers") {
            steps {
                script {
                    try {
                        sh 'docker rm -f $(docker ps -a -q)'
                    } catch (Exception e) {
                        echo 'No running container to clear up...'
                    }
                }
            }
        }
        stage("Start Docker") {
            steps {
                sh 'docker compose ps'
            }
        }
        stage("Run Composer Install") {
            steps {
                sh 'composer update && composer install'
                sh 'cp .env.example .env'
                sh 'docker compose run --rm artisan test'
            }
        }                 
   }
}
