pipeline {
    agent any
    options {
        timeout(time: 20, unit: 'MINUTES')
    }
    stages {
        // NPM dependencies
        stage('pull npm dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('build Docker Image') {
            steps {
                script {
                    // build image
                    docker.build("429718069504.dkr.ecr.us-east-1.amazonaws.com/netflix-oct:v1")
               }
            }
        }
        stage('Trivy Scan (Aqua)') {
            steps {
                sh 'trivy image --format template --output trivy_report.html 429718069504.dkr.ecr.us-east-1.amazonaws.com/netflix-oct:v1'
            }
        }
        stage('Push to ECR') {
            steps {
                script {
                    docker.withRegistry('https://429718069504.dkr.ecr.us-east-1.amazonaws.com/netflix-oct:v1', 'ecr:us-east-1:sylviewn-ecr') {
                        // build image
                        def myImage = docker.build("429718069504.dkr.ecr.us-east-1.amazonaws.com/netflix-oct:v1")
                        // push image
                        myImage.push()
                    }
                }
            }
        }
    }
}

