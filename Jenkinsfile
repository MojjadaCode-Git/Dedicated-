pipeline {
    agent any

    tools {
        maven 'Maven3'     // Jenkins tool name for Maven
        jdk 'Java11'       // Jenkins tool name for JDK 11
    }

    environment {
        NEXUS_URL = 'http://<your-server-ip>:8081/repository/maven-releases/'
        NEXUS_CREDENTIALS = credentials('nexus-credentials') // Add in Jenkins
        TOMCAT_SERVER = 'http://<your-server-ip>:8080/manager/text'
        TOMCAT_CREDENTIALS = credentials('tomcat-credentials') // Add in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/MojjadaCode-Git/Dedicated-.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Upload to Nexus') {
            steps {
                sh """
                mvn deploy:deploy-file \
                  -DgroupId=com.example \
                  -DartifactId=demo \
                  -Dversion=1.0-SNAPSHOT \
                  -Dpackaging=war \
                  -Dfile=target/demo-1.0-SNAPSHOT.war \
                  -DrepositoryId=nexus \
                  -Durl=$NEXUS_URL \
                  -Dusername=$NEXUS_CREDENTIALS_USR \
                  -Dpassword=$NEXUS_CREDENTIALS_PSW
                """
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                sh """
                curl -u $TOMCAT_CREDENTIALS_USR:$TOMCAT_CREDENTIALS_PSW \
                  -T target/demo-1.0-SNAPSHOT.war \
                  "$TOMCAT_SERVER/deploy?path=/demo&update=true"
                """
            }
        }
    }
}
