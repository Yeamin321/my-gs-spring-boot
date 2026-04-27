pipeline {
    agent any

    tools {
        maven '3.9.11'
    }

    environment {
        NEXUS_URL = 'http://localhost:8081/repository/maven-releases/'
        NEXUS_USER = 'admin'
        NEXUS_PASS = 'PASSWORD'
    }

    stages {
        stage('Build JAR') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Archive JAR') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }

        stage('Upload JAR to Nexus') {
            steps {
                sh '''
                JAR_FILE=$(ls target/*.jar | grep -v original | head -n 1)

                curl -u $NEXUS_USER:$NEXUS_PASS \
                --upload-file $JAR_FILE \
                $NEXUS_URL$(basename $JAR_FILE)
                '''
            }
        }
    }
}
