pipeline {
    
    agent any

        environment {
        IMAGE_REPO_NAME = "test-repo"
        IMAGE_TAG = "${env.BUILD_NUMBER}"
        AWS_DEFAULT_REGION = "ap-south-1"
        AWS_ACCOUNT_ID = "792262496906"
        REPOSITORY_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}"
        NAME = "my-task-defination"
    }

    stages {
        
        stage('Logging & into AWS ECR') {
            steps {
                script {
                    sh "sudo aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/l7p5p7u0"
                    sh "sudo docker build -t test-repo ."
                    sh "sudo docker tag test-repo:latest public.ecr.aws/l7p5p7u0/test-repo:latest"
                    sh "sudo docker push public.ecr.aws/l7p5p7u0/test-repo:latest"

                }
            }
        }
        
        stage('Deploy to Fargate') {
            steps {
                script {
                    sh "sudo aws ecs update-service --cluster  my-nginx-cluster --service my-service --task-definition my-task-defination:2 --region ap-south-1 --force-new-deployment"
                }
            }
        }

        
    
    }
}
