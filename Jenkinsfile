pipeline {
    agent any 

    environment {
        IMAGE_NAME = "2022BCD0031/sample-app"
        CONTAINER_NAME = "sample-app-container"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/your-repo/sample-app.git' // Replace with your repo URL
            }
        }

        stage('Build') {
            steps {
                echo 'Building the application...'
                sh 'mvn clean package' // Replace with your build command (e.g., npm install, make, etc.)
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'mvn test' // Modify based on your tech stack (e.g., pytest, jest, etc.)
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo 'Running SonarQube analysis...'
                sh 'mvn sonar:sonar -Dsonar.host.url=http://localhost:9000 -Dsonar.login=your-token'
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Building Docker image...'
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }

        stage('Docker Push') {
            steps {
                echo 'Pushing Docker image to registry...'
                sh "docker login -u your-username -p your-password"
                sh "docker push ${IMAGE_NAME}"
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                sh "docker run -d --name ${CONTAINER_NAME} -p 8080:8080 ${IMAGE_NAME}"
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
