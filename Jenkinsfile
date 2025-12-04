pipeline {
    agent any

    tools {
        maven 'Maven3'
        jdk 'JDK17'
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
                dir('complete') {    // ВАЖЛИВО: pom.xml у папці complete
                    sh 'mvn clean install'
                }
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'complete/target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success {
            echo "Build SUCCESS!"
        }
        failure {
            echo "Build FAILED!"
        }
    }
}
