pipeline {
    agent any

    environment {
        GIT_REPO = 'https://github.com/sundar474/spring-boot-mongo-docker.git'
        MAVEN_HOME = '/var/lib/jenkins/tools/hudson.tasks.Maven_MavenInstallation/Maven-3.8.6'
        SONARQUBE_URL = 'http://43.205.232.134:9000/'
        DOCKER_REGISTRY = 'https://hub.docker.com/u/saint473'
        DOCKER_CRED_ID = 'Docker_Hub_Cred'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: GIT_REPO
            }
        }

        stage('Build') {
            steps {
                script {
                    def mavenCmd = "${env.MAVEN_HOME}/bin/mvn"
                    sh "${mavenCmd} clean package"
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                 withSonarQubeEnv('Sonar Server-7.8') {
                     script {
                        def mavenCmd = "${env.MAVEN_HOME}/bin/mvn"
                        sh "${mavenCmd} sonar:sonar"
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t saint473/spring-boot-mongo ."
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry("${DOCKER_REGISTRY}", DOCKER_CRED_ID) {
                        def customImage = docker.image("saint473/spring-boot-mongo")
                        customImage.push()
                    }
                }
            }
        }
    }
}
