pipeline {
    agent any

    triggers {
        pollSCM('H/5 * * * *') // simulates "trigger: main"
    }

    environment {
        MAVEN_OPTS = "-Xmx3072m"
        JAVA_HOME = "/usr/lib/jvm/java-11-openjdk"
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

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package -X'
            }
        }

stage('Run Tests') {
    steps {
        sh 'mvn clean test'
    }
}

stage('Publish Test Results') {
    steps {
        junit allowEmptyResults: true, testResults: '**/target/surefire-reports/TEST-*.xml'
    }
}
    }

    post {
        success {
            echo 'Build SUCCESS ✔'
        }
        failure {
            echo 'Build FAILED ❌'
        }
    }
}
