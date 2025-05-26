pipeline {
    agent any

    environment {
        MAVEN_HOME = tool 'Maven 3'
        DOCKER_IMAGE = 'aishwaryakm1106/java-demo-app'
    }

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'github-https', url: 'https://github.com/aishwaryak515/jenkins-maven-sample.git'
            }
        }

        stage('Build') {
            steps {
                sh "${MAVEN_HOME}/bin/mvn clean package"
            }
        }

        stage('SonarQube Scan') {
            environment {
                SONAR_USER_HOME = "${env.WORKSPACE}/.sonar"
            }
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh "${MAVEN_HOME}/bin/mvn sonar:sonar"
                }
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    dockerImage = docker.build("${DOCKER_IMAGE}:${BUILD_NUMBER}")
                }
            }
        }

        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh """
                        echo "$PASSWORD" | docker login -u "$USERNAME" --password-stdin
                        docker push ${DOCKER_IMAGE}:${BUILD_NUMBER}
                        docker tag ${DOCKER_IMAGE}:${BUILD_NUMBER} ${DOCKER_IMAGE}:latest
                        docker push ${DOCKER_IMAGE}:latest
                    """
                }
            }
        }
    }

    post {
        success {
            echo '✅ CI/CD Pipeline executed successfully!'
        }
        failure {
            echo '❌ CI/CD Pipeline failed!'
        }
    }
}
