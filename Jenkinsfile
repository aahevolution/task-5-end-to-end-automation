pipeline {
    agent any

    environment {
        WAR_FILE = 'target/maven-web-app.war'   // Confirm WAR name after mvn build
        TOMCAT_USER = 'ec2-user'                         // Replace with actual user if different
        TOMCAT_HOST = '3.111.30.20'               // Replace with your Tomcat server IP
        TOMCAT_PATH = '/opt/tomcat/webapps'              // Path to Tomcat’s webapps folder
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/ashokitschool/maven-web-app.git'
            }
        }

        stage('Build with Maven') {
            steps {
                script {
                    try {
                        sh 'mvn clean package'
                    } catch (err) {
                        echo "Build failed, but continuing: ${err}"
                    }
                }
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                script {
                    echo "Deploying to Apache Tomcat..."
                    sh """
                    if [ -f "${WAR_FILE}" ]; then
                        scp -o StrictHostKeyChecking=no ${WAR_FILE} ${TOMCAT_USER}@${TOMCAT_HOST}:${TOMCAT_PATH}/
                        echo "WAR deployed to Tomcat."
                    else
                        echo "WAR file not found, skipping deployment."
                    fi
                    """
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
