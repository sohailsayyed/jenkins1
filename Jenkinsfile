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
                    expression { env.BRANCH_NAME.startsWith("release/") } 
                }
            }
    
            steps {
                echo "${env.BRANCH_NAME}"
                echo "!!!!!!!!!!!!!!@@@@@@@@@@@@Started>>>>>>>>>>>>>!!!!!!!!!!!!!!!!!!!"
            }
        }

        stage('Deploy to S3') {
            when {
                anyOf{
                    expression { env.BRANCH_NAME == "develop" }
                    expression { env.BRANCH_NAME == "main" }
                    expression { env.BRANCH_NAME.startsWith("release/") }
                }
            }

            steps {
                script {
                    if (env.BRANCH_NAME == 'develop') {
                        sh 'pwd'
                        sh 'aws s3 sync . s3://test.spchavan.link/'
                        //aws cloudfront create-invalidation --distribution-id E2EJJPGYAV8659 --paths /*  
                    } else if (env.BRANCH_NAME == 'main') {
                        sh 'pwd'
                        sh 'aws s3 sync . s3://help.spchavan.link/'
                        //aws cloudfront create-invalidation --distribution-id E84HX4RZRQNW3 --paths /*
                    } else if (env.BRANCH_NAME.startsWith("release/")) {
                        sh 'pwd'
                        sh 'aws s3 sync . s3://help.spchavan.link/'
                        //aws cloudfront create-invalidation --distribution-id E84HX4RZRQNW3 --paths /*
                    }
                }

                echo "+++Upload Successful+++"
            }
        }
    }
}
