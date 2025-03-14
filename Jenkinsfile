pipeline {
    agent any

    tools {
        // Use predefined tool names from Jenkins Global Tool Configuration
        jdk 'jdk17'          // Name of JDK installation in Jenkins
        maven 'maven3.9'     // Name of Maven installation in Jenkins
    }

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds') // Jenkins credentials ID for Docker Hub
        NEXUS_REPO_URL = 'http://your-nexus-ip:8081/repository/maven-releases/'
    }

    stages {
        stage('GIT Checkout') {
            steps {
                git branch: 'main', 
                url: 'https://github.com/HabibBenJdidia/DevOps.git'
            }
        }

        stage('Build Application') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("habibbenjdidia/timesheet-app:${env.BUILD_ID}")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-creds') {
                        dockerImage.push()
                    }
                }
            }
        }

        stage('Deploy to Nexus') {
            steps {
                sh '''
                mvn deploy:deploy-file \
                  -DgroupId=com.example \
                  -DartifactId=timesheet-devops \
                  -Dversion=1.0.0 \
                  -Dpackaging=jar \
                  -Dfile=target/*.jar \
                  -DrepositoryId=nexus \
                  -Durl=${NEXUS_REPO_URL}
                '''
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                sh 'docker-compose down && docker-compose up -d'
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
