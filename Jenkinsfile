pipeline {
    agent any

    tools {
        maven 'MAVEN_3'      // Configure Maven under Jenkins -> Global Tool Configuration
        jdk 'JAVA_17'        // Configure JDK 17 under Jenkins -> Global Tool Configuration
    }

    environment {
        // Nexus
        NEXUS_URL = "http://54.166.204.73:8082/repository/maven-releases/"
        NEXUS_USER = "admin"
        NEXUS_PASS = "Virat@2025!"

        // Tomcat
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
                sh 'mvn clean package -DskipTests'
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
                  -DrepositoryLayout=default \
                  -DgeneratePom=true \
                  -DuniqueVersion=true \
                  -Dusername=$NEXUS_USER \
                  -Dpassword=$NEXUS_PASS
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

    post {
        success {
            echo "🎉 Build, Nexus upload, and Tomcat deployment successful!"
        }
        failure {
            echo "❌ Build or deployment failed. Check logs."
        }
    }
}
