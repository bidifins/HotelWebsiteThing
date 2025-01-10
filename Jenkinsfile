pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    echo "Cloning repository..."
                    // Clone the 'main' branch
                    git branch: 'main', url: 'https://github.com/bidifins/HotelWebsiteThing.git'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker image for the main branch..."
                    bat "docker rmi hotel-website-image-main:latest || exit 0"
                    bat "docker build -t hotel-website-image-main ."
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Use a fixed port for the main branch
                    def branchPort = 3008

                    echo "Stopping and removing any existing container for the main branch..."
                    bat "docker stop hotel-website-container-main || exit 0"
                    bat "docker rm hotel-website-container-main || exit 0"

                    echo "Running new container for the main branch on port ${branchPort}..."
                    bat "docker run --rm -d --name hotel-website-container-main -p ${branchPort}:80 hotel-website-image-main"

                    echo "Container for the main branch is running on port ${branchPort}"
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        always {
            echo 'Pipeline completed!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
