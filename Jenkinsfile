pipeline {
    agent any
    options {
        timeout(time: 20, unit: 'MINUTES')
    }
    stages{
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
                    docker.build("335871625378.dkr.ecr.eastus.amazonaws.com/netflix-oct:latest")
               }
            }
        }
        stage('Trivy Scan (Aqua)') {
            steps {
                sh 'trivy image --format template --output trivy_report.html 335871625378.dkr.ecr.eastus.amazonaws.com/netflix-oct:v1'
            }
       }
        stage('Push to ECR') {
            steps {
                script{
                    //https://<AwsAccountNumber>.dkr.ecr.<region>.amazonaws.com/netflix-app', 'ecr:<region>:<credentialsId>
                    docker.withRegistry('https://335871625378.dkr.ecr.eastus.amazonaws.com/netflix-oct', 'ecr:eastus:karo-ecr') {
                    // build image
                    def myImage = docker.build("335871625378.dkr.ecr.eastus.amazonaws.com/netflix-oct:v1")
                    // push image
                    myImage.push()
                    }
                }
            }
        }
        
    }
}
