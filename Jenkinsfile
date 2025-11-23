pipeline {
    agent any

    environment {
        // AWS / ECR 정보 (과제에서 사용한 값 기준 예시)
        AWS_REGION   = "ap-northeast-2"
        ECR_REGISTRY = "189115277514.dkr.ecr.ap-northeast-2.amazonaws.com"

        FRONT_REPO   = "ddd-front"
        BACK1_REPO   = "ddd1-back1"
        BACK2_REPO   = "ddd1-back2"

        // 빌드 번호를 태그로 사용 (예: build-1, build-2 ...)
        IMAGE_TAG    = "build-${env.BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                echo "[Checkout] Git에서 소스 가져오기"
                checkout scm
            }
        }

        stage('Docker Build') {
            steps {
                echo "[Docker Build] 아래 명령으로 이미지 빌드한다고 가정"
                echo "  docker build -t $ECR_REGISTRY/$FRONT_REPO:$IMAGE_TAG ./frontend"
                echo "  docker build -t $ECR_REGISTRY/$BACK1_REPO:$IMAGE_TAG ./backend1"
                echo "  docker build -t $ECR_REGISTRY/$BACK2_REPO:$IMAGE_TAG ./backend2"
            }
        }

        stage('ECR Push') {
            steps {
                echo "[ECR Push] 아래 명령으로 ECR 로그인 + 이미지 푸시한다고 가정"
                echo "  aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $ECR_REGISTRY"
                echo "  docker push $ECR_REGISTRY/$FRONT_REPO:$IMAGE_TAG"
                echo "  docker push $ECR_REGISTRY/$BACK1_REPO:$IMAGE_TAG"
                echo "  docker push $ECR_REGISTRY/$BACK2_REPO:$IMAGE_TAG"
            }
        }

        stage('ECS Deploy') {
            steps {
                echo "[ECS Deploy] ECS 서비스에 강제 재배포한다고 가정"
                echo "  aws ecs update-service --cluster <ECS_CLUSTER_NAME> --service <FRONT_SERVICE_NAME> --force-new-deployment --region $AWS_REGION"
                echo "  aws ecs update-service --cluster <ECS_CLUSTER_NAME> --service <BACK1_SERVICE_NAME> --force-new-deployment --region $AWS_REGION"
                echo "  aws ecs update-service --cluster <ECS_CLUSTER_NAM
