pipeline { 
agent any 
environment { 
NODE_VERSION = '22' 
} 
stages { 
stage('Checkout') { 
steps { 
// Specify the branch explicitly 
                git branch: 'main', url: '' 
            } 
        } 
       stage('Code Analysis') { 
            environment { 
                scannerHome = tool 'Sonarqube-test' 
            } 
            steps { 
                script { 
                    withSonarQubeEnv('SonarQubeServer22BCD31') { 
                        bat "${scannerHome}/bin/sonar-scanner \ 
                            -Dsonar.projectKey=Todo-APP22BCD31\ 
                            -Dsonar.projectName=Todo-APP22BCD31 \ 
                            -Dsonar.sources=. " 
                    } 
                } 
            } 
        } 
        stage('Build and Start Services') { 
            steps { 
                bat 'docker-compose up --build -d' 
            } 
        } 
        stage('Run Backend Tests') { 
            steps { 
                bat ''' 
                    docker exec backend-service npm install 
                
                ''' 
            } 
        } 
        stage('Run Frontend Tests') { 
            steps { 
                bat ''' 
                    docker exec frontend-service npm install 
                ''' 
            } 
        } 
        stage('Deploy') { 
            when { 
                branch 'main' 
            } 
            steps { 
                echo "Deploying the application..." 
                bat 'docker-compose up -d' 
            } 
        } 
    } 
    post { 
        success { 
            echo "Pipeline executed successfully!" 
        } 
        failure { 
            echo "Build or tests failed!" 
            bat 'docker-compose down' 
        } 
    } 
}
