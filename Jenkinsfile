pipeline {

    agent {
        kubernetes {
            yaml '''
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: maven
    image: maven:3.9.9-eclipse-temurin-21
    command: ['cat']
    tty: true
'''
        }
    }

    stages {

        stage('Build') {
            steps {
                container('maven') {
                    dir('demo/demo') {
                    sh 'mvn --version'
                    sh 'mvn clean package'
                }}
            }
        }
    }
}
