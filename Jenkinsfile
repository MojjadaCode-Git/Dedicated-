
pipeline {
    agent any

    tools {
        maven 'MAVEN_3'      // Jenkins → Global Tool Configuration lo set chesina Maven
        jdk 'Java17'         // Jenkins → Global Tool Configuration lo set chesina JDK
    }

    environment {
        NEXUS_URL = "http://54.166.204.73:8082/repository/maven-releases/"
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
                sh 'mvn clean deploy'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                sh '''
                echo "Deploying to Tomcat..."
                WAR_FILE=target/demo.war
                if [ -f "$WAR_FILE" ]; then
                  curl -u $TOMCAT_USER:$TOMCAT_PASS -T $WAR_FILE \
                    "$TOMCAT_URL/deploy?path=/demo&update=true"
                else
                  echo "WAR file not found!"
                  exit 1
                fi
                '''
            }
        }
    }
}
