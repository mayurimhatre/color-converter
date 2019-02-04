pipeline {
    agent { 
        docker { 
            image 'node:9'
            args '-u 0:0'
        }
    }
    environment {
        NODE_PATH = 'var/lib/jenkins/plugins/out/agent/nodejs'
        IASTAGENT_REMOTE_ENDPOINT_HTTP_ENABLED = 'true'
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
                    // forever doesn't even have a '-r' so the process is starting without the agent attached.
                    // sh "forever start -e err.log -r agent_nodejs_linux64 app/server.js"
                    sh 'pwd'
                    sh 'ls -al'
                    sh "forever start -e err.log -c 'node -r agent_nodejs_linux64' app/server.js"
                    sh 'cat err.log'
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