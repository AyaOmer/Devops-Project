pipeline {
    agent any

    // environment {
        
        // DOCKER_IMAGE = "your-dockerhub-username/devops-project:${env.BUILD_NUMBER}"
      // REGISTRY_CREDENTIALS = 'dockerhub-credentials-id' // Replace with your Docker Hub credentials ID in Jenkins
    // }

    stages {

        stage('Install Dependencies') {
            steps {
                sh 'apt-get install npm '
            }
        }

        stage('Build') {
            steps {
                // Build the React application
                sh 'npm run build'
            }
        }

        stage('Test') {
            steps {
                // Run tests
                sh 'npm test -- --coverage'
            }
            post {
                always {
                    // Archive test results and coverage reports
                    junit 'coverage/junit.xml'
                    publishHTML(target: [
                        allowMissing: false,
                        alwaysLinkToLastBuild: true,
                        keepAll: true,
                        reportDir: 'coverage',
                        reportFiles: 'index.html',
                        reportName: 'Coverage Report'
                    ])
                }
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    // Build Docker image using Dockerfile in Docker directory
                    docker.build("${DOCKER_IMAGE}", "-f Docker/Dockerfile .")
                }
            }
        }

        // stage('Docker Push') {
        //     steps {
        //         script {
        //             // Login to Docker Hub
        //           //  docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-credentials-id') {
        //                 // Push the Docker image
        //              //   docker.image("${DOCKER_IMAGE}").push()
        //             }
        //         }
        //     }
         }

       
    

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}