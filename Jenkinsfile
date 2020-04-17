node {
    def commit_id
    stage('Preparation') {
        checkout scm
        sh "git rev-parse --short HEAD > .git/commit-id"                        
        commit_id = readFile('.git/commit-id').trim()
    }
    stage('test') {
        nodejs(nodeJSInstallationName: 'nodejs') {
            sh 'npm install --only=dev' // only install dev dependency
            sh 'npm test'
        }
    }
    stage('docker build/push') {
        //connect to docker hub with dockerhub credential present in jenkins
        docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
            //tag the app with the github repo and commit
            def app = docker.build("8b-Berto/hello-express:${commit_id}", '.').push()
        }
    }
}