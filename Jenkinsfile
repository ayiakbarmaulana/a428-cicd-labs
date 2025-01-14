node {
  docker.image('node:16-buster-slim').withRun('-p 3000:3000') {container => 
    stage('Build') {
      sh 'npm install'
    }
  }
}