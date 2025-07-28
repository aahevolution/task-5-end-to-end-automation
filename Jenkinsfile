pipeline {
    agent any

    environment {
        TOMCAT_PATH = "/opt/tomcat/webapps"
        WAR_NAME = "maven-web-app.war"
    }

    stages {
        stage('Clone') {
            steps {
                echo 'Cloning repository...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building WAR using Maven...'
                sh 'mvn clean package'
            }
            post {
                failure {
                    echo '❌ Build failed, continuing to Deploy stage anyway...'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    def warFile = "target/${env.WAR_NAME}"
                    if (fileExists(warFile)) {
                        echo "Deploying WAR to Tomcat..."
                        sh "sudo cp ${warFile} ${TOMCAT_PATH}/"
                    } else {
                        echo "⚠️ WAR file not found: ${warFile}, skipping deployment"
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed.'
        }
    }
}
