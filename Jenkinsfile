pipeline {
    agent any
  
    environment {
        DOCKERHUB_CREDENTIALS=credentials('dockerhub')
      }

    stages {
        stage('Checkout') {
              steps {
                  git url: 'https://github.com/adeoyedewale/ray-repo.git'
              }
         }
      
        stage("Build Frontend") {
            steps {
                sh "cd frontend && npm install && npm run build"
            }
        }

        stage("Build Backend") {
            steps {
                sh "cd backend && npm install && npm run build"
            }
        }

        stage("Build Docker Images") {
            steps {
                sh "docker build -t rayfrontend -f frontend/Dockerfile frontend"
                sh "docker build -t raybackend -f backend/Dockerfile backend"
            }
        }
      stage('Push Images to Docker Hub') {
            steps {
                //sh 'docker login -u $DOCKER_HUB_USERNAME -p $DOCKER_HUB_PASSWORD'
                sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
                sh "docker push $DOCKER_USERNAME/rayfrontend:$BUILD_NUMBER"
                sh "docker push $DOCKER_USERNAME/raybackend:$BUILD_NUMBER"
                //sh 'docker push frontend:latest'
                //sh 'docker push backend:latest'
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
