pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'npm install'
        sh 'CI=false npm run build'
      }
    }
    stage('Deploy') {
      steps {
        sshPublisher(
          publishers: [
            sshPublisherDesc(
              configName: 'test-app',
              transfers: [
                sshTransfer(
                  sourceFiles: 'build/**,Dockerfile,nginx.conf',
                  remoteDirectory: '/opt/frontend',
                  execCommand: '''
                    cd opt/frontend
                    docker stop frontend || true &&
                    docker rm frontend || true &&
                    docker build -t frontend .
                    docker run -d --name frontend -p 80:80 -p 443:443 frontend:${BUILD_NUMBER}
                    echo $?
                  '''
                )
              ]
            )
          ]
        )
      }
    }
  }
}