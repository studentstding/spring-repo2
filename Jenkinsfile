pipeline {

    agent any

    tools {
        maven 'Maven3'
    }

    stages {

        stage('Build') {
            steps {
                sh 'mvn --version'
                sh 'mvn clean package'
            }
        }
    }
}
