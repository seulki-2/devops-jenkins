pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Hello') {
            steps {
                echo 'Jenkins Pipeline 연결 성공! by Seulki'
            }
        }

        stage('List Files') {
            steps {
                sh 'pwd'
                sh 'ls -al'
            }
        }
    }
}

