pipeline {
    agent any

    tools {
        jdk 'JAVA_HOME'
        maven 'M2_HOME'
    }

    stages {

        stage('GIT') {
            steps {
                git branch: 'main', url: 'https://github.com/ademselmani/atelier-devops.git'
            }
        }

        stage('Compile Stage') {
            steps {
                sh 'mvn clean compile'
            }
        }

        // stage('MVN SONARQUBE') {
        //     steps {
        //         sh "mvn sonar:sonar -Dsonar.login=squ_c0931e4b9fc970410f5037c889771f1f9db8c76f -Dmaven.test.skip=true"
        //     }
        // }

        stage('Deploy to Nexus') {
            steps {
                sh 'mvn deploy -Dmaven.test.skip=true'
            }
        }
    }
}
