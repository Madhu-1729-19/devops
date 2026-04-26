pipeline {
    agent any   // uses default Jenkins node (master or available agent)

    tools {
        jdk 'JDK11'
        maven 'Maven3'
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Madhu-1729-19/devops.git'
            }
        }

        stage('Build with Maven') {
            steps {
                bat 'mvn clean package'
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
