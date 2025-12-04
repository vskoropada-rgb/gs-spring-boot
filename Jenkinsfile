pipeline {
    agent any

    environment {
        TELEGRAM_TOKEN = credentials('TELEGRAM_TOKEN')
        TELEGRAM_CHAT_ID = credentials('TELEGRAM_CHAT_ID')
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/vskoropada-rgb/gs-spring-boot.git'
            }
        }

        stage('Build') {
            steps {
                sh "mvn clean install"
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
