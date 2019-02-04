pipeline {
    agent { 
        docker { 
            image 'node:9'
            args '-u 0:0'
        }
    }
    environment {
        NODE_PATH = '/agent'
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
                // echo sh(returnStdout: true, script: 'env')
                script {
                    def agentPath = "${NODE_PATH}/agent_nodejs_linux64.node"
                    dir("${NODE_PATH}") {
                        if (fileExists("agent_nodejs_linux64.node")) {
                        echo "Using Agent: ${agentPath}"
                        } else {
                        echo "ERROR: Agent cannot be found at: ${agentPath}"
                        }
                    }
                }
                sh 'pwd'
                wrap([$class: 'HailstoneBuildWrapper', location: 'localhost', port: '10010']) {
                    // sh "forever start -e err.log -r agent_nodejs_linux64 app/server.js"
                    sh "forever start -e err.log -c 'node -r agent_nodejs_linux64 app/server.js"
                    //sh 'forever start -e err.log --killSignal SIGTERM --minUptime 1000 --spinSleepTime 1000 -c /bin/sh ./start.sh'
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