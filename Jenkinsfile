pipeline {
    agent any

    tools {
        maven 'maven3'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/aishwaryak515/jenkins-maven-sample.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Test Report') {
            steps {
                junit '**/target/surefire-reports/*.xml'
            }
        }
    }
    post {
        always {
            echo 'Build finished!'
        }
    }
}
