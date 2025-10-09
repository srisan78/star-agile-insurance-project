pipeline {
    agent any
    tools {
        maven 'mymaven'
    }
    stages {
        stage('Checkout Code') {
            steps {
                echo 'Checking out code from Git repository'
                git 'https://github.com/srisan78/star-agile-insurance-project.git'
            }
        }
        stage('Test Code') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Build Application') {
            steps {
                echo "Building application..."
                sh "mvn clean package"
            }
        }
        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image'
                sh 'docker build -t myinsurance:$BUILD_NUMBER .'
            }
        }
        stage('Push Docker Image') {
            steps {
                withCredentials([string(credentialsId: 'DOCKER_HUB_PASWD', variable: 'DOCKER_HUB_PASWD')]) {
                    sh 'docker login -u sridhar76 -p $DOCKER_HUB_PASWD'
                }
                sh 'docker tag myinsurance:$BUILD_NUMBER sridhar76/myinsurance:$BUILD_NUMBER'
                sh 'docker push sridhar76/myinsurance:$BUILD_NUMBER'
            }
        }
        stage('Run Docker Container') {
            steps {
                echo 'Running Docker container with port mapping'
                sh 'docker run -d -p 9080:9190 --name myinsurance_container sridhar76/myinsurance:$BUILD_NUMBER'
            }
        }
    }
}
