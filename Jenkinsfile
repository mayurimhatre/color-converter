pipeline {
    agent { 
        docker { 
            image 'node:9'
            args '-u 0:0'
        }
    }
    environment {
        NODE_PATH = '/home/mhama02/Documents/Hailstone/out/agent/nodejs'
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
                wrap([$class: 'HailstoneBuildWrapper', location: 'host.docker.internal', port: '10010']) {
                    sh 'node -r agent_nodejs_linux64 app.js'
                }
            }
        }
    }
}