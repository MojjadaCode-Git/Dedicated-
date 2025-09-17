pipeline {
    agent any

    tools {
        jdk 'Java17'
        maven 'MAVEN_3'
    }

    environment {
        NEXUS_URL = "http://54.166.204.73:8082/repository/maven-snapshots/"
        NEXUS_USER = "admin"
        NEXUS_PASS = "Virat@2025!"
        TOMCAT_USER = "tomcat"
        TOMCAT_PASS = "tomcat"
        TOMCAT_URL = "http://54.166.204.73:8081/manager/text"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/MojjadaCode-Git/Dedicated-.git'
            }
        }

        stage('Build & Deploy to Nexus') {
            steps {
                sh 'mvn clean deploy -DskipTests'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                sh '''
                echo "Deploying to Tomcat..."
                curl -u $TOMCAT_USER:$TOMCAT_PASS -T target/demo.war \
                  "$TOMCAT_URL/deploy?path=/demo&update=true"
                '''
            }
        }
    }
}
