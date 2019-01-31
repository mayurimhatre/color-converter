pipeline {
    agent { 
        docker { 
            image 'node:9'
            args '-e npm_config_cache=npm-cache'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh ''
               sh 'npm install'
               sh 'npm install forever -g'
            }
        }
        stage('Test'){
            steps {
                sh 'forever start app/server.js'
                sh 'npm test'
            }
        }
    }
}