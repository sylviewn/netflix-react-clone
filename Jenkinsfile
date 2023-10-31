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
                    docker.build("429718069504.dkr.ecr.us-east-1.amazonaws.com/netflix-oct:latest")
               }
            }
        }
        stage('Trivy Scan (Aqua)') {
            steps {
                sh 'trivy image --format template --output trivy_report.html 429718069504.dkr.ecr.us-east-1.amazonaws.com/netflix-oct:latest'
            }
       }
        stage('Push to ECR') {
            steps {
                script{
                    //https://<AwsAccountNumber>.dkr.ecr.<region>.amazonaws.com/netflix-app', 'ecr:<region>:<credentialsId>
                    docker.withRegistry('https://429718069504.dkr.ecr.us-east-1.amazonaws.com/netflix-oct:latest', 'ecr:us-east-1:karo-ecr') {
                    // build image
                    def myImage = docker.build("429718069504.dkr.ecr.us-east-1.amazonaws.com/netflix-oct:latest")
                    // push image
                    myImage.push()
                    }
                }
            }
        }
        
    }
}
