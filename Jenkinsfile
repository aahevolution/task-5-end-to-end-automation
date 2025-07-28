pipeline {
    agent any

    stages {
        stage('SCM Integration') {
            steps {
                echo "--- Stage: SCM Integration ---"
                checkout scm
            }
        }

        stage('Build Application') {
            steps {
                echo "--- Stage: Build Application ---"
                script {
                    try {
                        sh 'mvn clean package'
                    } catch (err) {
                        echo "Build failed but continuing..."
                        currentBuild.result = 'UNSTABLE'
                    }
                }
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                echo "--- Stage: Deploy to Tomcat ---"
                sh '''
                WAR_FILE=$(ls target/*.war | head -n 1)
                if [ -f "$WAR_FILE" ]; then
                    cp "$WAR_FILE" /opt/tomcat/webapps/
                    /opt/tomcat/bin/shutdown.sh || true
                    /opt/tomcat/bin/startup.sh
                else
                    echo "WAR file not found. Skipping deploy."
                fi
                '''
            }
        }
    }

    post {
        always {
            echo "--- Post Build Actions ---"
            echo "Pipeline finished with status: ${currentBuild.currentResult}"
        }
    }
}
