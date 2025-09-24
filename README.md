Firstly i have created VM and install Docker, Jenkins, Java21, I create one directory, in that directory I have Created 2 files, Dockerfile, and index.html.

<img width="574" height="451" alt="Screenshot (137)" src="https://github.com/user-attachments/assets/5fdb2fd3-b170-4416-ad8a-96545c91389f" />

give jenkins permission to docker.

install jenkins plugin,  create credential in Jenkins.then create pipeline.

pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "nginx-demo:${BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Anushree602/nginx-demo-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t $DOCKER_IMAGE ."
            }
        }

        stage('Deploy Nginx') {
            steps {
                sh """
                docker rm -f nginx-app || true
                docker run -d -p 80:80 --name nginx-app $DOCKER_IMAGE
                """
            }
        }
    }

    post {
        success { echo "✅ Nginx site deployed successfully!" }
        failure { echo "❌ Pipeline failed. Check logs." }
    }
}

<img width="1366" height="768" alt="Screenshot (135)" src="https://github.com/user-attachments/assets/7430d042-1a7f-4f4b-9392-386d67c88af2" />




