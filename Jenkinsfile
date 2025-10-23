pipeline {
    agent any

    environment {
        BASE_URL = "http://127.0.0.1:5000/"
    }

    stages {

        stage('Install Dependencies & Run Selenium Tests') {
            steps {
                echo "Installing dependencies and running Selenium Tests"
                bat 'pip install -r requirements.txt'
                // Start Flask app in background
                bat 'start /B python app.py'
                // Wait for server to start
                bat 'ping 127.0.0.1 -n 5 > nul'
                // Run pytest
                bat 'pytest -v --maxfail=1 --disable-warnings'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker Image"
                bat 'docker build -t lakshyabandi25/week12:t5 .'
            }
        }

        stage('Docker Login') {
            steps {
                bat 'docker login -u lakshyabandi25 -p Lakshya.bandi'
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                echo "Pushing Docker Image to Docker Hub"
                bat 'docker push lakshyabandi25/week12:t5'
            }
        }

        stage('Deploy to Kubernetes') { 
            steps { 
                bat 'kubectl apply -f deployment.yaml --validate=false' 
                bat 'kubectl apply -f service.yaml' 
            } 
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check the logs.'
        }
    }
}
