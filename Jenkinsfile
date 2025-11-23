pipeline {
    agent any

    environment {
        // AWS / ECR 정보 (예시 값, 필요하면 나중에 수정)
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
                echo "[Docker Build] Docker 이미지 빌드 단계 (논리만 표현)"
                echo "  docker build -t ${ECR_REGISTRY}/${FRONT_REPO}:${IMAGE_TAG} ./frontend"
                echo "  docker build -t ${ECR_REGISTRY}/${BACK1_REPO}:${IMAGE_TAG} ./backend1"
                echo "  docker build -t ${ECR_REGISTRY}/${BACK2_REPO}:${IMAGE_TAG} ./backend2"
            }
        }

        stage('ECR Push') {
            steps {
                echo "[ECR Push] ECR 푸시 단계 (논리만 표현)"
                echo "  aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${ECR_REGISTRY}"
                echo "  docker push ${ECR_REGISTRY}/${FRONT_REPO}:${IMAGE_TAG}"
                echo "  docker push ${ECR_REGISTRY}/${BACK1_REPO}:${IMAGE_TAG}"
                echo "  docker push ${ECR_REGISTRY}/${BACK2_REPO}:${IMAGE_TAG}"
            }
        }

        stage('ECS Deploy') {
            steps {
                echo "[ECS Deploy] ECS 서비스 강제 재배포 단계 (논리만 표현)"
                echo "  aws ecs update-service --cluster YOUR_ECS_CLUSTER --service FRONTEND_SERVICE --force-new-deployment --region ${AWS_REGION}"
                echo "  aws ecs update-service --cluster YOUR_ECS_CLUSTER --service BACKEND1_SERVICE --force-new-deployment --region ${AWS_REGION}"
                echo "  aws ecs update-service --cluster YOUR_ECS_CLUSTER --service BACKEND2_SERVICE --force-new-deployment --region ${AWS_REGION}"
                echo "  (YOUR_ECS_CLUSTER / *_SERVICE 부분은 실제 클러스터/서비스 이름으로 바꾸면 됨)"
            }
        }
    }
}
