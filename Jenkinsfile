pipeline {
    agent { docker { image 'node:9' } }
    environment {
        HOME = '.'
    }
    stages {
        stage('Build') {
            steps {
               sh 'npm install'
               sh 'npm install forever -g'
            }
        }
        stage('Test'){
            steps {
                sh 'forever start app/server.js'
                sh 'npm test'
                sh 'forever stop 0'
            }
        }
    }
}