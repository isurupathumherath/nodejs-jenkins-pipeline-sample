node {
    def commit_id

    stage('Preparation') {
        checkout scm
        sh "git rebase origin/main"
        sh "git rev-parse --short HEAD > .git/commit-id"

        commit_id = readFile('.git/commit-id').trim()
    }

    stage('Test') {
        nodejs(nodeJSInstallationName: 'nodejs') {
            sh 'npm install'
            sh 'npm test'
        }
    }

    stage('Docker Build and Push') {
        docker.withRegistry('https://index.docker.io/', 'dockerhub') {

            def app = docker.build("isuruherath22923/docker-example")
            app.push("${env.BUILD_NUMBER}")            
            app.push("latest")



            // def app = docker.build("isuruherath22923/docker-example:${commit_id}", '.').push()
            // def app = sh "docker build -t isuruherath22923/docker-example:${commit_id} ." 
            // app.push("${commit_id}")            
            // app.push("latest")
        }
    }
}