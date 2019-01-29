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
            }
        }
        stage('Test'){
            steps {
                sh '/usr/local/lib/node_modules/forever/bin/forever start app/server.js'
                sh 'npm test'
                sh 'forever stop 0'
            }
        }
    }
}