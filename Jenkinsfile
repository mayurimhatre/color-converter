pipeline {
    agent { 
        docker { 
            image 'node:9'
            args '-u 0:0'
        }
    }
    environment {
        NODE_PATH = '/srv/iast-agent'
        IASTAGENT_LOGGING_STDERR_LEVEL = 'debug'
        IASTAGENT_LOGGING_FILE_ENABLED = 'true'
        IASTAGENT_LOGGING_FILE_PATHNAME = 'iastdebug.txt'
        IASTAGENT_LOGGING_FILE_LEVEL = 'debug'
        IASTAGENT_ANNOTATIONHANDLER_JSONFILE_ENABLED = 'true'
        IASTAGENT_ANNOTATIONHANDLER_JSONFILE_PATHNAME = 'iastoutput.ndjson'
        IASTAGENT_ANNOTATIONHANDLER_JSONFILE_LEVEL = 'info'
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
                    sh "forever start -l ${BUILD_TAG}.log -o ${BUILD_TAG}-out.log -e ${BUILD_TAG}-err.log -c 'node -r agent_nodejs_linux64' app/server.js"
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