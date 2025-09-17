pipeline {
    agent any

    tools {
        maven "MAVEN_3"
        jdk "Java17"
    }

    environment {
        TOMCAT_URL = "http://54.166.204.73:8081/manager/text"
        TOMCAT_USER = "tomcat"
        TOMCAT_PASS = "tomcat"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/MojjadaCode-Git/Dedicated-.git'
            }
        }

        stage('Build & Deploy to Nexus') {
            steps {
                sh 'mvn clean deploy'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                sh '''
                    echo "Deploying to Tomcat..."
                    WAR_FILE=$(ls target/*.war | head -n 1)
                    curl -u $TOMCAT_USER:$TOMCAT_PASS -T $WAR_FILE "$TOMCAT_URL/deploy?path=/demo&update=true"
                '''
            }
        }
    }
}
