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
      command:
        - cat
      tty: true
      resources:
        requests:
          cpu: 100m
          memory: 256Mi
'''
        }
    }

    stages {
        stage('Debug') {
            steps {
                container('maven') {
                    sh 'pwd'
                    sh 'find . -name pom.xml'
                }
            }
        }

        stage('Build') {
            steps {
                container('maven') {
                    dir('demo/demo') {
                        sh 'mvn clean package'
                    }
                }
            }
        }

        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'demo/demo/target/*.jar',
                                 fingerprint: true
            }
        }
    }
}
