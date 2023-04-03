pipeline {
	
	agent any
	stages {

		stage ('Build') {

			steps {
				sh 'printenv'
			}
		}

		stage (''Publish ECR) {

			steps {

				with.Env (["AWS_ACCESS_KEY_ID=${env.AWS_ACCESS_KEY_ID}", "AWS_SECRET_ACCESS_KEY=${env.AWS_SECRET_ACCESS_KEY}", "AWS_DEFAULT_REGION=${env.AWS_DEFAULT_REGION}"]) {

					sh 'docker login -u AWS -p $(aws ecr-public get-login-password --region us-east-1) public.ecr.aws/l7p5p7u0'
					sh 'docker build -t test-repo .'
					sh 'docker tag test-repo:latest public.ecr.aws/l7p5p7u0/test-repo:latest'
					sh 'docker push public.ecr.aws/l7p5p7u0/test-repo:latest'

				}

			}
		}

	}


}
