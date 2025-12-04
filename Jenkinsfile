pipeline {
    agent any

    tools {
        maven 'Maven3'
        jdk 'JDK17'
    }

    environment {
        TELEGRAM_TOKEN = credentials('TELEGRAM_TOKEN')
        TELEGRAM_CHAT_ID = credentials('TELEGRAM_CHAT_ID')
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/vskoropada-rgb/gs-spring-boot.git'
            }
        }

        stage('Build') {
            steps {
                dir('complete') {             // ← працюємо всередині папки complete
                    sh "mvn clean install"
                }
            }
        }

        stage('Archive Artifact') {
            steps {
                dir('complete/target') {
                    archiveArtifacts artifacts: '*.jar', fingerprint: true
                }
            }
        }
    }

    post {
        success {
            sh """
            curl -s -X POST https://api.telegram.org/bot${TELEGRAM_TOKEN}/sendMessage \
                -d chat_id=${TELEGRAM_CHAT_ID} \
                -d text="✅ SUCCESS: Job '${JOB_NAME}' Build #${BUILD_NUMBER} completed successfully!"
            """
        }
        failure {
            sh """
            curl -s -X POST https://api.telegram.org/bot${TELEGRAM_TOKEN}/sendMessage \
                -d chat_id=${TELEGRAM_CHAT_ID} \
                -d text="❌ FAILURE: Job '${JOB_NAME}' Build #${BUILD_NUMBER} failed!"
            """
        }
    }
}
