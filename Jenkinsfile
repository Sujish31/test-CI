pipeline {
    agent any

    environment {
        S3_BUCKET = 'dev-artifact-bucket-diozutfp' // Replace with your S3 bucket name
        AWS_REGION = 'ap-south-1'       // Replace with your AWS region, e.g., us-east-1
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Cloning the GitHub repository
                git branch: 'main', url: 'https://github.com/Sujish31/test-CI.git' // Replace with your GitHub repo
            }
        }

        stage('Upload to S3') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-credentials-id' // Replace with your AWS credentials ID in Jenkins
                ]]) {
                    // Sync repository content to S3
                    sh '''
                    aws s3 sync . s3://${S3_BUCKET} --region ${AWS_REGION}
                    '''
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline execution completed."
        }
        success {
            echo "Files successfully uploaded to S3 bucket: ${S3_BUCKET}"
        }
        failure {
            echo "Pipeline failed."
        }
    }
}
