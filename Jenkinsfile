pipeline {
    agent { 
        docker { 
            image 'node:9'
            args '-u 0:0'
        }
    }
    environment {
        NODE_PATH = '.'
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
                sh 'pwd'
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
                    //*** sh "forever start -l ${BUILD_TAG}.log -o ${BUILD_TAG}-out.log -e ${BUILD_TAG}-err.log -c 'node -r srv/iast-agent/agent_nodejs_linux64' app/server.js"
                    sh "forever start -l ${BUILD_TAG}.log -o ${BUILD_TAG}-out.log -e ${BUILD_TAG}-err.log --killSignal SIGTERM --minUptime 1000 --spinSleepTime 1000 -c /bin/sh ./start.sh localhost 10010"
                    sleep(time:30,unit:"SECONDS")
                    script {
                        if (fileExists("${BUILD_TAG}.log")) {
                            sh "cat ${BUILD_TAG}.log"
                        }
                        if (fileExists("${BUILD_TAG}-out.log")) {
                            sh "cat ${BUILD_TAG}-out.log"
                        }
                        if (fileExists("${BUILD_TAG}-err.log")) {
                            sh "cat ${BUILD_TAG}-err.log"
                        }
                    }
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