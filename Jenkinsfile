node() {
    try {
        docker.image('node:16-buster-slim').inside("-p 3000:3000") {
            stage('Build') {
                sh 'npm install'
            }
        }
    } catch (err) {
        currentBuild.result = 'FAILURE'
        throw err
    }
}
