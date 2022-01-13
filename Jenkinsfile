node {
    def commit_id

    stage('Preparation') {
        checkout scm
        sh "git rebase origin/main"
        sh "git rev-parse --short HEAD > .git/commit-id"

        commit_id = readFile('.git/commit-id').trim()
    }

    stage('Test') {
        nodejs(nodeJsInstallationName: 'nodejs') {
            sh 'npm install'
            sh 'npm test'
        }
    }

    stage('Docker Build and Push') {
        docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
            def app = docker.build('isuruherath22923/docker-example:${commit_id}', '.').push()
        }
    }
}