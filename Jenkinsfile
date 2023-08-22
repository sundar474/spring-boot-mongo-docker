pipeline {
    agent any

    environment {
        GIT_REPO = 'https://github.com/sundar474/spring-boot-mongo-docker.git'
        MAVEN_HOME = '/usr/share/maven'
        SONARQUBE_URL = 'http://43.205.232.134:9000/'
        DOCKER_REGISTRY = 'https://index.docker.io/v1/'  // Corrected Docker registry URL
        DOCKER_CRED_ID = 'Docker_Hub_Cred'
        KUBE_CONFIG_CRED_ID = 'Kube_Config_Cred'  // Your Kubernetes configuration credentials ID
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

        stage('Deploy Application In Kubernetes Cluster') {
            steps {
                withKubeConfig(credentialsId: KUBE_CONFIG_CRED_ID) {
                    sh "kubectl apply -f springBootMongo.yml"
                }
            }
        }
    }
}
