pipeline {
    agent any

    tools {
        maven 'MAVEN_3'     // Jenkins Tools config lo petti undali
        jdk 'Java17'        // Jenkins Tools config lo petti undali
    }

    environment {
        NEXUS_URL = "http://54.166.204.73:8082/repository/maven-snapshots/"
        NEXUS_ID  = "nexus"   // settings.xml lo <id> match avvali
        TOMCAT_USER = "tomcat"
        TOMCAT_PASS = "tomcat"
        TOMCAT_URL  = "http://54.166.204.73:8081/manager/text"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/MojjadaCode-Git/Dedicated-.git'
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
                echo "Deploying WAR to Tomcat..."
                WAR_FILE=$(ls target/*.war | head -n 1)
                echo "Deploying $WAR_FILE"
                curl -u $TOMCAT_USER:$TOMCAT_PASS -T $WAR_FILE \
                  "$TOMCAT_URL/deploy?path=/demo&update=true"
                '''
            }
        }
    }
}
