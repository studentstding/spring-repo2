pipeline {

    agent any

    tools {
        maven 'Maven3'
    }

    stages {

        stage('Build') {
            steps {
                dir('demo/demo') {
                sh 'mvn --version'
                sh 'mvn clean package'
                }
            }
        }
    }
}
