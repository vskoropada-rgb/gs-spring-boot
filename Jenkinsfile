pipeline {
    agent any

    environment {
        TELEGRAM_TOKEN = credentials('TELEGRAM_TOKEN')
        TELEGRAM_CHAT_ID = credentials('TELEGRAM_CHAT_ID')
    }

    stages {

        stage('Build') {
            steps {
                sh "mvn clean install"
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
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
