node {
    try {
        checkout scm
        docker.image('node:16-buster-slim').inside("-p 3000:3000") {
            stage('Build') {
                sh 'npm install'
            }
            stage('Test') {
                sh './jenkins/scripts/test.sh'
            }
            stage('Manual Approval') {
                input message: 'Lanjutkan ke tahap Deploy?'
            }
            stage('Deploy') {
                sshagent(credentials: ['a54789ce-6d79-4114-a78c-1dff917fec83']) {
                    sh '''
                    # Ensure the .ssh directory exists
                    mkdir -p ~/.ssh

                    # Add the EC2 instance to known_hosts
                    echo "Adding EC2 instance to known_hosts"
                    ssh-keyscan -H ec2-13-215-173-108.ap-southeast-1.compute.amazonaws.com >> ~/.ssh/known_hosts
                    '''
                    sh '''
                    echo "Deploying to EC2 instance"
                    ssh ubuntu@ec2-13-215-173-108.ap-southeast-1.compute.amazonaws.com "cd ~/a428-cicd-labs && git pull && whoami && npm install && ./jenkins/scripts/deliver.sh"
                    '''
                }
                input message: 'Finished using the website? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
    } catch (err) {
        currentBuild.result = 'FAILURE'
        throw err
    }
}
