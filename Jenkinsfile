node {

   def commit_id
   def app
   
   stage('Preparation') {
     checkout scm
     sh "git rev-parse --short HEAD > .git/commit-id"                        
     commit_id = readFile('.git/commit-id').trim()
   }
   stage('test') {
     nodejs(nodeJSInstallationName: 'nodejs') {
       sh 'npm install --only=dev'
       sh 'npm test'
     }
   }
   stage('docker build') {
       app = docker.build("isuruherath22923/docker-example:${commit_id}", '.')
   }

   stage('Push image') {
    docker.withRegistry('https://registry-1.docker.io/v2/', 'dockerhub') {
      app.push()
    }
  }
}