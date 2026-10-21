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
  - name: kaniko
    image: gcr.io/kaniko-project/executor:v1.23.2
    command:
    - sleep
    args:
    - "999999"  
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

        stage('Build Image') {
    steps {
        container('kaniko') {
            sh '''
            /kaniko/executor \
              --context=`pwd`/demo/demo \
              --dockerfile=`pwd`/demo/demo/Dockerfile \
              --no-push
            '''
        }
    }
}
    }
}
