pipeline {
    agent any

    triggers {
        pollSCM('H/5 * * * *')
    }

    environment {
        MAVEN_OPTS = "-Xmx3072m"
        JAVA_HOME = "/usr/lib/jvm/java-11-openjdk"
        APP_NAME = "devops"
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

        stage('Verify WAR File') {
            steps {
                sh 'ls -lh target/'
            }
        }

        stage('Archive WAR (Jenkins Storage - SSP Style)') {
            steps {
                archiveArtifacts artifacts: 'target/*.war', fingerprint: true
            }
        }

        stage('Run Tests') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Publish Test Results') {
            steps {
                junit allowEmptyResults: true,
                      testResults: '**/target/surefire-reports/TEST-*.xml'
            }
        }

        stage('Deploy WAR to Tomcat') {
            steps {
                bat '''
                echo Deploying WAR to Tomcat...

                copy target\\*.war C:\\ProgramData\\chocolatey\\lib\\tomcat\\tools\\apache-tomcat\\webapps\\%APP_NAME%.war

                echo Deployment Completed ✔
                '''
            }
        }

        stage('App URL') {
            steps {
                echo "================================="
                echo "APP DEPLOYED SUCCESSFULLY ✔"
                echo "OPEN URL:"
                echo "http://localhost:8080/devops"
                echo "================================="
            }
        }
    }

    post {
        success {
            echo 'BUILD + DEPLOY SUCCESS ✔'
        }
        failure {
            echo 'BUILD FAILED ❌'
        }
    }
}
