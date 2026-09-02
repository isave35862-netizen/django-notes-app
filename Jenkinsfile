@Library('Shared') _

pipeline {
    agent { label 'vinod' }

    triggers {
        githubPush()
    }

    stages {
        stage('Hello') {
            steps {
                // calls vars/jenkins.groovy
                jenkins()
            }
        }

        stage('Debug') {
            steps {
                echo 'Checking environment before build...'
                sh 'docker --version || echo "Docker not found"'
                sh 'docker-compose --version || echo "Docker Compose not found"'
                sh 'ls -l'
            }
        }

        stage('Code') {
            steps {
                script {
                    echo 'Cloning the code'
                    clone('https://github.com/isave35862-netizen/django-notes-app.git', 'main')
                    echo 'Code cloning successful'
                }
            }
        }

        stage('Build') {
            steps {
                echo 'Building the Docker image'
                dockerBuild('notes-app', 'latest', 'rajratnaisave')
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests'
                // Add your test commands here
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying with docker-compose'
                // Corrected syntax: "docker-compose down && docker-compose up ..."
                sh 'docker-compose down && docker-compose up -d --build'
            }
        }

        stage('Docker Push') {
            steps {
                script {
                    docker_push('notes-app', 'latest', 'rajratnaisave')
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished (success or failure)'
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed — check logs above for details.'
        }
    }
}
