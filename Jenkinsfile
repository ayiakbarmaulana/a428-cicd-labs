node {
    def app = docker.image('node:16-buster-slim')
    
    stage('Run Container') {
        app.run('-p 3000:3000') // Memulai container
    }
    
    stage('Build') {
        app.inside {
            sh 'npm install'
        }
    }
}