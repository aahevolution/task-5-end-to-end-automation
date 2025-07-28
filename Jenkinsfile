pipeline {
    agent { label 'linux-agent' }  // match your agent label
    stages {
        stage('Build') {
            steps {
                echo 'Building on slave agent...'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
            }
        }
    }
}
