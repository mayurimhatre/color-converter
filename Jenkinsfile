pipeline {
    agent { 
        docker { 
            image 'node:9'
            args '-u 0:0 --net=host'
        }
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
                wrap([$class: 'HailstoneBuildWrapper', location: 'localhost', port: '10010']) {
                    echo "IASTAGENT_REMOTE_ENDPOINT_HTTP_ENABLED=${env.IASTAGENT_REMOTE_ENDPOINT_HTTP_ENABLED}"
                    echo "IASTAGENT_REMOTE_ENDPOINT_HTTP_LOCATION=${env.IASTAGENT_REMOTE_ENDPOINT_HTTP_LOCATION}"
                    echo "IASTAGENT_REMOTE_ENDPOINT_HTTP_PORT=${env.IASTAGENT_REMOTE_ENDPOINT_HTTP_PORT}"
                    sh 'export DEBUG=1 && curl -sSL https://s3.us-east-2.amazonaws.com/app.hailstone.io/iast-ci.sh | sh'
                    echo sh(returnStdout: true, script: 'env')
                    sh "forever start -l ${BUILD_TAG}.log -o ${BUILD_TAG}-out.log -e ${BUILD_TAG}-err.log --killSignal SIGTERM --minUptime 1000 --spinSleepTime 1000 -c 'node -r ./agent_nodejs_linux64' app/server.js"
                    // sh "forever start -l ${BUILD_TAG}.log -o ${BUILD_TAG}-out.log -e ${BUILD_TAG}-err.log --killSignal SIGTERM --minUptime 1000 --spinSleepTime 1000 -c /bin/sh ./start.sh"
                    sleep(time:30,unit:"SECONDS")
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