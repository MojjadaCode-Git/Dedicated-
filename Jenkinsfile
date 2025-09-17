pipeline {
    agent any

    tools {
        maven 'MAVEN_3'   // name configured in Jenkins
    }

    environment {
        NEXUS_URL = "http://54.166.204.73:8082/repository/maven-releases/"
        NEXUS_CREDENTIALS = credentials('nexus-creds')  // ID in Jenkins credentials
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

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Upload to Nexus') {
            steps {
                sh '''
                mvn deploy:deploy-file \
                  -DgroupId=com.example \
                  -DartifactId=demo \
                  -Dversion=1.0.0 \
                  -Dpackaging=war \
                  -Dfile=target/demo.war \
                  -DrepositoryId=nexus \
                  -Durl=$NEXUS_URL \
                  -Dnexus.username=$NEXUS_CREDENTIALS_USR \
                  -Dnexus.password=$NEXUS_CREDENTIALS_PSW
                '''
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
