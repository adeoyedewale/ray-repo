pipeline {
    agent any
  
    environment {
        DOCKERHUB_CREDENTIALS=credentials('dockerhub')
	AWS_DEFAULT_REGION="us-east-1"
	THE_BUTLER_SAYS_SO=credentials('aws-cred4')
      }

    stages {
        stage('Checkout') {
              steps {
                  git url: 'https://github.com/adeoyedewale/ray-repo.git'
              }
         }
      
        stage("Build Frontend") {
            steps {
                sh "cd frontend && npm ci && npm install && npm run build"
		sh "cd .."
            }
        }

        stage("Build Backend") {
            steps {
                sh "cd backend && npm ci && npm install"
		sh "cd .."
            }
        }

        stage("Build Docker Images") {
            steps {
                //sh "docker build -t $DOCKER_USERNAME/rayfrontend:$BUILD_NUMBER -f frontend/Dockerfile ./frontend"
		sh "docker build -t $DOCKER_USERNAME/rayfrontend:$BUILD_NUMBER ./frontend"
		sh "cd .."
                //sh "docker build -t $DOCKER_USERNAME/raybackend:$BUILD_NUMBER -f backend/Dockerfile ./backend"
		sh "docker build -t $DOCKER_USERNAME/raybackend:$BUILD_NUMBER -f backend/Dockerfile ./backend"
            }
        }
      /*stage('Push Images to Docker Hub') {
            steps {
                //sh 'docker login -u $DOCKER_HUB_USERNAME -p $DOCKER_HUB_PASSWORD'
                sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
                sh "docker push $DOCKER_USERNAME/rayfrontend:$BUILD_NUMBER"
                sh "docker push $DOCKER_USERNAME/raybackend:$BUILD_NUMBER"
                //sh 'docker push frontend:latest'
                //sh 'docker push backend:latest'
            }
        }*/
	    
	 stage('Push') {
	      steps {
		      withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
			      sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
			      sh "docker push $DOCKER_USERNAME/rayfrontend:$BUILD_NUMBER"
			      sh "docker push $DOCKER_USERNAME/raybackend:$BUILD_NUMBER"
		      }
	      }
      }
	    
	 stage ('Build and Publish to ECR') {
	      steps {
		      sh '''
		      aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/c6p1p1z3
		      docker tag eruobodo/devops-challenge-image:$BUILD_NUMBER public.ecr.aws/c6p1p1z3/newrepo:$BUILD_NUMBER
		      docker push public.ecr.aws/c6p1p1z3/rayfrontend:$BUILD_NUMBER
		      docker push public.ecr.aws/c6p1p1z3/backend:$BUILD_NUMBER
		      '''
	      }
      } 
    }
  
  post {
        always {
	        cleanWs()
      	  	sh 'docker logout'
        }
   }
}
