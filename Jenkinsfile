pipeline {
    agent any
    triggers { pollSCM('* * * * *') }
    stages {
        stage('Checkout') {
            steps {
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
                docker build -t xahmedmahmoudx/iti-g111:ahmedmahmoud .
                docker images -a
                docker push xahmedmahmoudx/iti-g111:ahmedmahmoud
                """)
            }
        }
    }
    post {
        always {
            junit '**/target/surefire-reports/TEST-*.xml'
            archiveArtifacts 'target/*.jar'
        }
    }
}