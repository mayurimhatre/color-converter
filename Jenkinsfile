pipeline {
    agent { 
        docker { 
            image 'node:9'
            args '-u 0:0'
        }
    }
    environment {
        NODE_PATH = '/srv/iast-agent'
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
                // sh 'pwd'
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
                wrap([$class: 'HailstoneBuildWrapper', location: 'localhost', port: '10010']) {
                    sh "forever start -l color-converter-log.log -o color-converter-out.log -e color-converter-error.log -c 'export NODE_PATH=/srv/iast-agent; node -r agent_nodejs_linux64' app/server.js"
                    sleep(time:30,unit:"SECONDS")
                    // sh 'cat color-converter-log.log'
                    // sh 'cat color-converter-out.log'
                    sh 'cat color-converter-error.log'
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