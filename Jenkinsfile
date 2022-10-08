#!/usr/bin/env groovy
// shebang tells most editors to treat as groovy (syntax highlights, formatting, etc)

pipeline {
    agent any
    triggers { pollSCM('* * * * *') }
    stages {
        stage('Checkout') {
            steps {
                cleanWs()
                checkout scm: [$class: 'GitSCM' ]
                echo "$GIT_BRANCH"
                sh 'docker images -a'
            }
        }
        stage('Testing') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Build') {
            steps {
                sh (script: """
                mvn clean package
                docker build -t xahmedmahmoudx/iti-g111:latest .
                docker images -a
                docker push xahmedmahmoudx/iti-g111:latest
                """, returnStdout: true)
            }
        }
    }
    // post after stages, for entire pipeline, is also an implicit step albeit with explicit config here, unlike implicit checkout stage
    post {
        always {
            junit '**/target/surefire-reports/TEST-*.xml'
            archiveArtifacts 'target/*.jar'
        }
    }
}