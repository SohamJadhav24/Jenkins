pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'python:3.9' 
        DOCKER_COMPOSE_FILE = 'docker-compose.yml'
    }
    

    stages {
	    stage('Clone Repository') {
            steps {
                git url: 'https://github.com/SohamJadhav24/Jenkins.git', branch: 'main', credentialsId: 'my-github-credentials'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t sohamjadhav24/JenkinsImg .'
                }
            }
        }
        stage('Run Django Tests') {
            steps {
                script {
                    docker.image('JenkinsImg').inside {
                        sh 'pip install -r requirements.txt'
                        sh 'python manage.py test'
                    }
                }
            }
        }
        stage('Run Django Server') {
            steps {
                script {
                    docker.image('JenkinsImg').inside {
                        sh 'pip install -r requirements.txt'
                        sh 'python manage.py runserver 0.0.0.0:8000'
                    }
                }
            }
        }

        // stage('Deploy') {
        //     steps {
        //         script {
        //             sh '/usr/local/bin/docker-compose -f docker-compose.yml up -d'             
		//         }
        //     }
        // }

    }
    post {
        always {
            cleanWs() // Clean workspace after build
        }
    }
}