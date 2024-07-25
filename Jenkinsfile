pipeline {
    agent any

    tools {
        maven 'Maven3'
    }

    environment {
        GIT_REPO = 'https://github.com/ajacdev/demo-app.git'
        GIT_BRANCH = 'main'
        scannerHome = tool 'Sonar'
        SONAR_PROJECT_KEY = 'demo-app-sonar'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: "${GIT_BRANCH}", url: "${GIT_REPO}"
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'Sonar'
                    withSonarQubeEnv('Sonar') {
                        sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=${SONAR_PROJECT_KEY}"
                    }
                }
            }
        }
        stage('Quality Gate') {
            steps {
                timeout(time: 1, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }

}

