pipeline {
    agent any
    tools {
        nodejs 'nodejs'
    }
    stages {
        stage('Build') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'Github', url: 'https://github.com/rajeshmamuddu/Project.git']])
                sh 'npm install'
                // sh 'npm run build'
            }
        }
        stage('Test') {
            steps {
                // sh 'npm run test'
                echo "Test"
            }
        }
       stage('Build Image') {
            steps { 
                sh 'docker build -t reactimage .'
                sh 'docker tag reactimage:latest rajeshreactimage/dev:latest'
            }    
       }
       stage('Docker login') {
           environment {
        DOCKER_IMAGE = "rajeshreactimage/dev:latest${BUILD_NUMBER}"
        // DOCKERFILE_LOCATION = "java-maven-sonar-argocd-helm-k8s/spring-boot-app/Dockerfile"
        REGISTRY_CREDENTIALS = credentials('docker-cred')
           }
      }
       stage('Deploy') {
            steps {  
                def dockerImage = docker.image("${DOCKER_IMAGE}")
                docker.withRegistry('https://index.docker.io/v1/', "docker-cred") {
                dockerImage.push()
               }
            }
         }
    }
}
