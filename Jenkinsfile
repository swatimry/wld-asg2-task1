pipeline {
    agent any
    tools{
      nodejs 'nodejs'
    }
    environment {
        NODEJS_HOME = 'C:\\Program Files\\nodejs' 
        SCANNER_PATH='C:\\Users\\ASUS\\Downloads\\sonar-scanner-cli-6.2.1.4610-windows-x64\\sonar-scanner-6.2.1.4610-windows-x64\\bin'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Installation') {
            steps {
                bat '''
                set PATH=%NODEJS_HOME%;%PATH%
                npm install
                '''
            }
        }
        stage('Build') {
            steps {
                bat '''
                set PATH=%NODEJS_HOME%;%PATH%
                npm run build
                '''
            }
        }

        stage('SonarQube Analysis') {
            environment {
                SONAR_TOKEN = credentials('sonar-token') 
            }
            steps {
                bat '''
                set PATH=%SCANNER_PATH%;%PATH%
                sonar-scanner -Dsonar.projectKey=wld-asg2-task1 ^
                              -Dsonar.projectName=wld-asg2-task1 ^
                              -Dsonar.sources=. ^
                              -Dsonar.host.url=http://localhost:9000 ^
                              -Dsonar.token=%SONAR_TOKEN%
                '''
            }
        }
    }
    post {
        success {
            echo 'Pipeline completed successfully'
        }
        failure {
            echo 'Pipeline failed'
        }
        always {
            echo 'This runs regardless of the result.'
        }
    }
}
