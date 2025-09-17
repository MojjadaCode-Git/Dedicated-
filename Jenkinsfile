pipeline {
    agent any

    tools {
        maven 'MAVEN_3'      // Jenkins -> Global Tool Configuration lo "MAVEN_3" ani configure cheyyali
        jdk 'Java17'         // Jenkins -> Global Tool Configuration lo Java 17 ki "Java17" ani name petti configure cheyyali
    }

    environment {
        NEXUS_URL = "http://54.166.204.73:8082/repository/maven-releases/"
        NEXUS_USER = "admin"
        NEXUS_PASS = "Virat@2025!"
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

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Upload to Nexus') {
            steps {
                sh '''
                WAR_FILE=target/demo-1.0.0.war
                if [ ! -f "$WAR_FILE" ]; then
                  echo "WAR file not found!"
                  exit 1
                fi

                mvn deploy:deploy-file \
                  -DgroupId=com.example \
                  -DartifactId=demo \
                  -Dversion=1.0.0 \
                  -Dpackaging=war \
                  -Dfile=$WAR_FILE \
                  -DrepositoryId=nexus \
                  -Durl=$NEXUS_URL \
                  -Dusername=$NEXUS_USER \
                  -Dpassword=$NEXUS_PASS
                '''
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                sh '''
                echo "Deploying to Tomcat..."
                WAR_FILE=target/demo-1.0.0.war
                if [ ! -f "$WAR_FILE" ]; then
                  echo "WAR file not found for Tomcat!"
                  exit 1
                fi

                curl -u $TOMCAT_USER:$TOMCAT_PASS -T $WAR_FILE \
                  "$TOMCAT_URL/deploy?path=/demo&update=true"
                '''
            }
        }
    }
}
