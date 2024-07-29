pipeline {
    agent any    

    stages {
	    stage('Clone Repository') {
            steps {
                git url: 'https://github.com/SohamJadhav24/Jenkins.git', branch: 'main', credentialsId: 'my-github-credentials'
            }
        }
       
        stage('Build and Push Docker Image') {
            steps {
                script {
                    // This step should not normally be used in your script. Consult the inline help for details.
                    withDockerRegistry(credentialsId: 'my-github-credentials', toolName: 'Docker') {
                        sh 'docker build -t sohamjadhav24/jenkins_docker:tag124 .'
                        sh 'docker push sohamjadhav24/jenkins_docker:tag123'
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