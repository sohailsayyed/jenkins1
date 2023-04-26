pipeline {
        
    agent any
   
    stages {
        
        stage('Set ENV for branch') {
            steps {
                script {
                    def commit = checkout scm
                    // we set BRANCH_NAME to make when { branch } syntax work without multibranch job
                    env.BRANCH_NAME = commit.GIT_BRANCH.replace('origin/', '')
                    echo "${env.BRANCH_NAME}"
                }
            }
        }

        
        stage ('Install dependencies & Build Project') {
            
            when {
                anyOf{
                    expression { env.BRANCH_NAME == "develop" }
                    expression { env.BRANCH_NAME == "main" }
                    //expression { env.BRANCH_NAME.startsWith("release/") }
                         
                }
            }
    
            steps {
                
                echo "${env.BRANCH_NAME}"
                echo "Starting"
               
            }
            
        }

        stage('Deploy to S3') {

             when {
                anyOf{

                    expression { env.BRANCH_NAME == "develop" }
                    expression { env.BRANCH_NAME == "main" }
                    //expression { env.BRANCH_NAME.startsWith("release/") }
                }
            }

            steps {

                if (env.BRANCH_NAME == 'develop') {
                    sh 'pwd'
                    sh 'aws s3 sync build/ s3://test.spchavan.link/'
                    //aws cloudfront create-invalidation --distribution-id E2H7O15KPBNKNS --paths /*  
                }
                
                else (env.BRANCH_NAME == 'main') {
                    sh 'pwd'
                    sh 'aws s3 sync build/ s3://help.spchavan.link/'
                   // aws cloudfront create-invalidation --distribution-id E15K5NAMM80YAM --paths /*

                } 
                
                echo "+++Upload Successful+++"
            }
        }
  
    }
}



