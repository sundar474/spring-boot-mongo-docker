pipeline {
    agent any
    
    environment {
        GIT_REPO = 'https://github.com/sundar474/java-web-app-docker.git'
        MAVEN_HOME = '/var/lib/jenkins/tools/hudson.tasks.Maven_MavenInstallation/Maven-3.8.6'
        NEXUS_URL = 'http://3.142.194.99:8081/'
        SONARQUBE_URL = 'http://3.145.92.245:9000/'
        DOCKER_REGISTRY = 'https://index.docker.io/v1/'
        DOCKER_CRED_ID = 'Hub_Docker_Cred'
        KUBE_CONFIG_CRED_ID = 'KubeConfigFile'
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
                withCredentials([string(credentialsId: DOCKER_CRED_ID, variable: 'Hub_Docker_Cred')]) {
                    script {
                        sh """
                        echo \$Hub_Docker_Cred | docker login -u saint473 --password-stdin ${DOCKER_REGISTRY}
                        docker push saint473/spring-boot-mongo
                        """
                    }
                }
            }
        }
        /**
        stage('Deploy Application In Kubernetes Cluster') {
            steps {
                withKubeConfig(credentialsId: KUBE_CONFIG_CRED_ID) {
                    // Correct the path to the Kubernetes YAML file
                    sh "kubectl apply -f springBootMongo.yml"
                    **/
                }
            }
        }
    }
}
