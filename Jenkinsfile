pipeline {
    agent { 
        docker { 
            image 'node:9'
            args '-u 0:0'
        }
    }
    environment {
        NODE_PATH = '/agent'
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
                    sh "forever start -e err.log -c 'NODE_PATH=${NODE_PATH} node -r agent_nodejs_linux64' app/server.js"
                    sleep(time:30,unit:"SECONDS")
                    sh 'forever list'
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