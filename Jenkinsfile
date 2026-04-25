pipeline {
    agent {
        label 'Madhu-1'   // same as your agent pool
    }

    tools {
        // Make sure this JDK is configured in Jenkins Global Tool Configuration
        jdk 'JDK11'
        maven 'Maven3'
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://your-repo-url.git'
            }
        }

        stage('Build with Maven') {
            steps {
                bat 'mvn clean package -Xmx3072m'
            }
        }

        stage('Test Results') {
            steps {
                junit '**/surefire-reports/TEST-*.xml'
            }
        }

        stage('Archive WAR') {
            steps {
                archiveArtifacts artifacts: '**/target/*.war', fingerprint: true
            }
        }
    }
}
