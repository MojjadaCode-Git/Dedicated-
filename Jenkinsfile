pipeline {
    agent any

    environment {
        // Nexus configuration
        NEXUS_URL = "http://54.166.204.73:8082/repository/maven-releases/"
        NEXUS_REPO = "maven-releases"
        NEXUS_GROUP = "com.example"
        NEXUS_ARTIFACT = "demo"
        NEXUS_VERSION = "1.0.0"

        // Tomcat configuration
        TOMCAT_USER = "tomcat"
        TOMCAT_PASS = "tomcat"
        TOMCAT_URL  = "http://54.166.204.73:8081/manager/text"
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
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Upload Artifact to Nexus') {
            steps {
                sh '''
                mvn deploy:deploy-file \
                    -DgroupId=$NEXUS_GROUP \
                    -DartifactId=$NEXUS_ARTIFACT \
                    -Dversion=$NEXUS_VERSION \
                    -Dpackaging=war \
                    -Dfile=target/demo-1.0-SNAPSHOT.war \
                    -DrepositoryId=$NEXUS_REPO \
                    -Durl=$NEXUS_URL
                '''
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                sh '''
                echo "Deploying to Tomcat..."
                curl -u $TOMCAT_USER:$TOMCAT_PASS \
                     -T target/demo-1.0-SNAPSHOT.war \
                     "$TOMCAT_URL/deploy?path=/demo&update=true"
                '''
            }
        }
    }
}
