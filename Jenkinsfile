@Library("jenkins_lib") _
pipeline {
    agent { label "ubuntu_agent" }

    stages {
        stage('Hello') {
            steps {
                script {
                    hello()
                }
            }
        }
        stage('Checkout Source Code') {
            steps {
                echo 'Cloning source code'
                script {
                    checkout("https://github.com/SudhirVS/Docker-Zero-to-Hero.git","main")
                }
            }
        }
        stage('Building the source code') {
            steps {
                echo 'Builing the source code'
                sh "pwd"
                sh "docker build -t python-app:latest examples/python-web-app/"
            }
        }
        stage('Push Docker Image t Dockerhub') {
            steps {
                echo 'Pushing Image'
                 withCredentials([usernamePassword(credentialsId: 'docker_hub_cred', 
                 usernameVariable: 'DOCKER_USER', 
                 passwordVariable: 'DOCKER_PASS')]) {
                        sh "docker login -u ${env.DOCKER_USER} -p ${env.DOCKER_PASS}"
                        sh "docker image tag python-app:latest ${env.DOCKER_USER}/python-app:latest"
                        sh "docker push ${env.DOCKER_USER}/python-app:latest"
                    }
                echo " docker push image successful"
            }
        }
        stage('Deployment Docker Build') {
            steps {
                echo 'Deploying the Docker Build'
                sh "pwd"
                sh "docker compose -f examples/python-web-app/docker-compose.yml up -d"
                echo 'Deployment Successful'
            }
        }    
        }
    }
