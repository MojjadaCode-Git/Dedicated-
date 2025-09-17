pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Upload to Nexus') {
            steps {
                sh 'mvn deploy'
            }
        }
        stage('Deploy to Tomcat') {
            steps {
                sh './scripts/deploy.sh'
            }
        }
    }
}

