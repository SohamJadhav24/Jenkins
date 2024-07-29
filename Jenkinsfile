pipeline {
    agent any
    tools{
        jdk 'OpenJDK8'
        maven 'Maven3'
    }
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
        stage('Test Docker') {
            steps {
                script {
                    def dockerHome = tool name: 'Docker', type: 'org.jenkinsci.plugins.docker.commons.tools.DockerTool'
                    sh "${dockerHome}/bin/docker --version"
                }
            }
        }
        stage('Build and Push Docker Image') {
            steps {
                script {
                    // This step should not normally be used in your script. Consult the inline help for details.
                    withDockerRegistry(credentialsId: 'my-github-credentials', toolName: 'Docker') {
                        sh 'docker build -t sohamjadhav24/jenkins_docker:tag123 .'
                        sh 'docker push'
                    }
                    
                }
            }
        }
        // stage('Run Django Tests') {
        //     steps {
        //         script {
        //             docker.image('JenkinsImg').inside {
        //                 sh 'pip install -r requirements.txt'
        //                 sh 'python manage.py test'
        //             }
        //         }
        //     }
        // }
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