pipeline {
  agent any

  environment {
    AWS_REGION   = "ap-northeast-2"
    ECR_REGISTRY = "189115277514.dkr.ecr.ap-northeast-2.amazonaws.com"
    FRONT_REPO   = "ddd-front"
    BACK1_REPO   = "ddd1-back1"
    BACK2_REPO   = "ddd1-back2"
    IMAGE_TAG    = "build-${env.BUILD_NUMBER}"
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
        echo "checkout ok"
      }
    }

    stage('Docker Build') {
      steps {
        echo "docker build -t ${ECR_REGISTRY}/${FRONT_REPO}:${IMAGE_TAG} ./frontend"
        echo "docker build -t ${ECR_REGISTRY}/${BACK1_REPO}:${IMAGE_TAG} ./backend1"
        echo "docker build -t ${ECR_REGISTRY}/${BACK2_REPO}:${IMAGE_TAG} ./backend2"
      }
    }

    stage('ECR Push') {
      steps {
        echo "aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${ECR_REGISTRY}"
        echo "docker push ${ECR_REGISTRY}/${FRONT_REPO}:${IMAGE_TAG}"
        echo "docker push ${ECR_REGISTRY}/${BACK1_REPO}:${IMAGE_TAG}"
        echo "docker push ${ECR_REGISTRY}/${BACK2_REPO}:${IMAGE_TAG}"
      }
    }

    stage('ECS Deploy') {
      steps {
        echo "aws ecs update-service --cluster YOUR_ECS_CLUSTER --service FRONTEND_SERVICE --force-new-deployment --region ${AWS_REGION}"
        echo "aws ecs update-service --cluster YOUR_ECS_CLUSTER --service BACKEND1_SERVICE --force-new-deployment --region ${AWS_REGION}"
        echo "aws ecs update-service --cluster YOUR_ECS_CLUSTER --service BACKEND2_SERVICE --force-new-deployment --region ${AWS_REGION}"
      }
    }
  }
}
