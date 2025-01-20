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
        }

        stage('Deploy') {
            sshagent(credentials: ['a54789ce-6d79-4114-a78c-1dff917fec83']) {
                sh '''
                # Ensure the .ssh directory exists
                mkdir -p ~/.ssh

                # Add the EC2 instance to known_hosts
                echo "Adding EC2 instance to known_hosts"
                ssh-keyscan -H ec2-3-1-211-213.ap-southeast-1.compute.amazonaws.com >> ~/.ssh/known_hosts
                '''
                sh '''
                echo "Deploying to EC2 instance"
                ssh ubuntu@ec2-3-1-211-213.ap-southeast-1.compute.amazonaws.com "whoami && cd ~/a428-cicd-labs && git pull && npm install && ./jenkins/scripts/deliver.sh"
                '''
            }
            
        }
    } catch (err) {
        currentBuild.result = 'FAILURE'
        throw err
    }
}
