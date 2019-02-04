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
                echo sh(returnStdout: true, script: 'env')
                wrap([$class: 'HailstoneBuildWrapper', location: 'localhost', port: '10010']) {
                    // sh "forever start -e err.log -r agent_nodejs_linux64 app/server.js"
                    sh 'forever stopall'
                    sh "forever start -e err.log -r agent_nodejs_linux64 app/server.js"
                    sh 'forever list'
                    sh 'cat err.log'
                     sh 'forever list'
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