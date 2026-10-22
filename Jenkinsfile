pipeline {
    parameters {
      text(
          name: "Message",
          defaultValue: "message default"
      )  
    choice(
        name: 'ENVIRONMENT',
        choices: ['dev', 'prod'],
        description: 'Gdzie wdrożyć aplikację'
    )
        
        string(
            name: 'param1',
            defaultValue: 'default val'
        )
    }
    
    agent {
        kubernetes {
            yaml '''
apiVersion: v1
kind: Pod
spec:
  containers:
    # Kontener 1: kompiluje aplikację
    - name: maven
      image: maven:3.9.9-eclipse-temurin-21
      command:
        - cat
      tty: true

    # Kontener 2: buduje obraz z Dockerfile
    - name: kaniko
      image: gcr.io/kaniko-project/executor:v1.23.2-debug
      command:
        - /busybox/cat
      tty: true
'''
        }
    }

    stages {
stage('print') {
    steps {
        sh "echo ${params.param1}"
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

        stage('Build Image') {
            steps {
                container('kaniko') {
                    sh '''
                        /kaniko/executor \
                          --context="${WORKSPACE}/demo/demo" \
                          --dockerfile="${WORKSPACE}/demo/demo/Dockerfile" \
                          --destination=europe-central2-docker.pkg.dev/mr-dev12/docker-repo/spring-demo:${BUILD_NUMBER}
                    '''
                }
            }
        }
    }
}
