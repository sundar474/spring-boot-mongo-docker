pipeline {
	agent any

	environment {
		Git_REPO = 'https://github.com/sundar474/spring-boot-mongo-docker.git'
		MAVEN_HOME = '/var/lib/jenkins/tools/hudson.tasks.Maven_MavenInstallation/Maven-3.8.6'
	}

	stages {
		stage('Checkout') {
			steps {
				git branch: 'master', url: Git_REPO
			}
		}
	}
