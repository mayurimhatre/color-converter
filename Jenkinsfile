pipeline {
    agent { 
        docker { 
            image 'node:9'
            args '-u 0:0'
        }
    }
    environment {
        CLASSPATH = '/home'
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
                echo sh(returnStdout: true, script: 'env')
                wrap([$class: 'HailstoneBuildWrapper', location: 'localhost', port: '10010']) {
                    sh "forever start -r agent_nodejs_linux64 app/server.js"
                    sh 'npm test'
                    sh 'forever stop 0'
                }
            }
        }
        stage('Deploy') { 
            steps {
                sh 'echo npm package would run here...'
            }
        }
    }
}