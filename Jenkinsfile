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
        stage ('Build Docker Image') {
            when {
                 anyOf {
                    expression { env.BRANCH_NAME == "main" }
                    expression { env.BRANCH_NAME == ~"feature" }
                    expression { env.BRANCH_NAME == "develop" }
                    expression { env.BRANCH_NAME ==~ "release" }
                }
            }
        
        steps {
            script {
                    sh "docker build -t ${IMAGE_REPO_NAME}:${IMAGE_TAG} ."
                }
            }
        }
        
        stage('Logging & into AWS ECR') {
            steps {
                script {
                    sh "aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/l7p5p7u0"
                    sh "docker build -t test-repo ."
                    sh "docker tag test-repo:latest public.ecr.aws/l7p5p7u0/test-repo:latest"
                    sh "docker push public.ecr.aws/l7p5p7u0/test-repo:latest"

                }
            }
        }
        
        stage('Deploy to Fargate') {
            steps {
                script {
                    sh "aws ecs update-service --cluster  my-nginx-cluster --service my-service --task-definition my-task-defination:2 --region ap-south-1 --force-new-deployment"
                }
            }
        }

        
    
    }
}
