pipeline{

    // Jenkins can run the pipeline on any available agent/node
    agent any

    // Define tools required for the pipeline
    tools{
        // Use Maven tool configured in Jenkins global tools
        maven 'mymaven'
    }

    stages{

        stage('clone the repo'){
            steps{
                // Clone source code from GitHub repository
                git branch: 'main', url: 'https://github.com/RaviTeja110820/Docker-Image-Build-using-Jenkins-CI-CD.git'
            }
        }

        stage('Build Code'){
            steps{
                // Build Java project using Maven
                // clean -> removes previous build artifacts
                // package -> creates WAR file
                sh 'mvn clean package'
            }
        }

        stage('Build Image'){
            steps{
                // Build Docker image from Dockerfile in repository
                // -t assigns a tag/name to the image
                sh 'docker build -t mydockerjenkins .'
            }
        }

        stage('login and push Image'){
            steps{

                // Use Jenkins credentials stored securely
                // DOCKERHUB_TOKEN is stored in Jenkins credentials manager
                withCredentials([string(credentialsId: 'DOCKERHUB_TOKEN', variable: 'DOCKERHUB_PASSWORD')]) {

                    // Login to DockerHub
                    // Password is taken securely from Jenkins credential store
                    sh 'docker login -u ravteja -p ${DOCKERHUB_PASSWORD}'
                }

                // Tag image with DockerHub repository name
                sh 'docker tag mydockerjenkins ravteja/mydockerjenkins'

                // Push image to DockerHub registry
                sh 'docker push ravteja/mydockerjenkins'
            }
        }

        stage('Deploy the Image'){
            steps{

                // Run the container in detached mode
                // -d runs container in background
                // -P maps container ports automatically to host ports
                sh 'docker run -d -P ravteja/mydockerjenkins'
            }
        }

    }
}