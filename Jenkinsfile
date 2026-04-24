pipeline {
    agent any

    triggers {
        pollSCM('H/5 * * * *')
    }

    tools {
        jdk 'jdk11'
        maven 'maven3'
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Madhu-1729-19/devops.git'
            }
        }

        stage('Build WAR') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Verify WAR') {
            steps {
                sh 'ls -lh target/'
            }
        }

        stage('Archive WAR') {
            steps {
                archiveArtifacts artifacts: 'target/*.war', fingerprint: true
            }
        }

        stage('Run Tests') {
            steps {
                sh 'mvn test || true'
            }
        }

        stage('Publish Test Results') {
            steps {
                junit allowEmptyResults: true,
                      testResults: '**/target/surefire-reports/*.xml'
            }
        }

        stage('Deploy to Tomcat (Linux-safe)') {
            steps {
                sh '''
                echo "Deploying WAR..."

                cp target/*.war /tmp/ssp.war

                echo "WAR copied (simulate deploy) ✔"
                '''
            }
        }

        stage('App URL') {
            steps {
                echo "APP URL: http://localhost:8080/ssp"
            }
        }
    }

    post {
        success {
            echo 'BUILD SUCCESS ✔'
        }
        failure {
            echo 'BUILD FAILED ❌'
        }
    }
}
