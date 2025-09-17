pipeline {
    agent any

    tools {
        maven 'MAVEN_3'   // Jenkins Global Tool Config -> Maven installation name
        jdk 'Java17'      // Jenkins Global Tool Config -> JDK 17 installation name
    }

    environment {
        NEXUS_URL = "http://54.166.204.73:8082/repository/maven-snapshots/"
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
                curl -u $TOMCAT_USER:$TOMCAT_PASS -T target/demo.war \
                  "$TOMCAT_URL/deploy?path=/demo&update=true"
                '''
            }
        }
    }
}
