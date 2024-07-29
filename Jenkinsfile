pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'python:3.9' 
    }
    stages {
	    stage('Clone Repository') {
            steps {
                git url: 'https://github.com/SohamJadhav24/Jenkins.git', branch: 'main', credentialsId: '2cc1bd6f-7a93-42b7-bd05-0d66bc88eb19'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build('DjangoCRM .')
                }
            }
        }
        stage('Run Django Tests') {
            steps {
                script {
                    docker.image('DjangoCRM').inside {
                        sh 'pip install -r requirements.txt'
                        sh 'python manage.py test'
                    }
                }
            }
        }
        stage('Run Django Server') {
            steps {
                script {
                    docker.image('DjangoCRM').inside {
                        sh 'pip install -r requirements.txt'
                        sh 'python manage.py runserver 0.0.0.0:8000'
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    sh 'docker-compose up -d'                 
		        }
            }
        }

    }
    post {
        always {
            cleanWs() // Clean workspace after build
        }
    }
}