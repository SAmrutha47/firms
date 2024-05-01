pipeline {
    agent any
    tools {
        jdk 'jdk17'
    }
    environment {
        SCANNER_HOME = tool 'sonar-scanner'
    }
    stages {
        stage('clean workspace') {
            steps {
                cleanWs()
            }
        }
        stage('Git Checkout') {
                    steps {
                        script {
                            git branch: 'main',
                                credentialsId: '0e60dcac-42a6-498c-83aa-d163cc4fa0e8',
                                url: 'https://github.com/username/repository.git'
                        }
                    }
                }
        stage("Sonarqube Analysis") {
            steps {
                withSonarQubeEnv('sonar-server') {
                    sh '''$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=FirmManagement \
                    -Dsonar.projectKey=Firm'''
                }
            }
        }
        stage("quality gate") {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'Sonar-token'
                }
            }
        }
    }
}