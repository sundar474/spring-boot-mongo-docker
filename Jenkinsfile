pipeline {
    agent any

    environment {
        GIT_REPO = 'https://github.com/sundar474/spring-boot-mongo-docker.git'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch 'master', url: GIT_REPO
            }
        }
    }
}
