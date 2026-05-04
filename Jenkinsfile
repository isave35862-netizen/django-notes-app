@Library("Raj-library") _
pipeline {
    agent { label 'vinod' }

    // Trigger pipeline on GitHub push events
    triggers {
        githubPush()
    }

    stages {
        stage('Hello') {
            steps {
                jenkins()   // calls vars/jenkins.groovy
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
                echo 'Cloning the code'
                cloneRepo('https://github.com/LondheShubham153/django-notes-app.git', 'main')
                echo 'Code cloning successful'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the Docker image'
                dockerBuild('notes-app', 'latest', 'trainwithshubham')
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests'
                // Add your test commands here, e.g. pytest or unit tests
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying with docker-compose'
                sh 'docker compose && down docker-compose up -d --build'
            }
        }

        stage('Docker Push') {
            steps {
                dockerPush('notes-app', 'latest', 'trainwithshubham')
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
