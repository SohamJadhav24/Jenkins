pipeline {
    agent {
        label 
\docker_agent\'
    }
    environment {
        DOCKER_IMAGE = \python:3.9\' 
    }
    stages {
                stage(\Build
Docker
Image\') {
            steps {
                script {
                    docker.build(\DjangoCRM\')
                }
            }
        }
        stage(\Run
Django
Tests\') {
            steps {
                script {
                    docker.image(\DjangoCRM\').inside {
                        sh \pip
install
-r
requirements.txt\'
                        sh \python
manage.py
test\'
                    }
                }
            }
        }
        stage(\Run
Django
Server\') {
            steps {
                script {
                    docker.image(\DjangoCRM\').inside {
                        sh \pip
install
-r
requirements.txt\'
                        sh \python
manage.py
runserver
0.0.0.0:8000\'
                    }
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
