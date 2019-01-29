pipeline {
    agent { docker { image 'node:9' } }
    environment {
        HOME = '.'
    }
    stages {
        stage('Build') {
            steps {
               sh 'npm install'
               sh 'npm install forever'
               sh 'forever start app/server.js'
            }
        }
        stage('Test'){
            steps {
                sh 'npm test'
                sh 'forever stop 0'
            }
        }
    }
}