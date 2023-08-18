pipeline {
    agent any
    
    environment {
        GIT_REPO = 'https://github.com/sundar474/spring-boot-mongo-docker.git'
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
