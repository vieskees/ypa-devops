pipeline {
    environment {
        DOCKER_IMAGE_NAME = "moduo/devops"
        DOCKER_IMAGE = "${DOCKER_IMAGE_NAME}:v${BUILD_NUMBER}"
    }
    agent {
        docker {
            image 'maven:3-alpine'
            args '-v /root/.m2:/root/.m2 -v /var/run/docker.sock:/var/run/docker.sock'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
                sh "docker build -t ${DOCKER_IMAGE} ."
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit allowEmptyResults: true, testResults: 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deploy') {
            steps {
                sh "docker push ${DOCKER_IMAGE}"
            }
        }
    }
}