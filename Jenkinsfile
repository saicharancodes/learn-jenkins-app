pipeline {
    agent any

    stages {
        stage('without-docker') {
            steps {
                sh ''' echo "this is running without the docker locally"
                touch withoutdocker.txt'''}}
        
        stage ("with the docker") {
            agent {
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }}
            steps{
                    echo "hello i'm running inside the dockerrr"
                    sh ''' npm --version
                    touch withdocker.txt
                    
                    '''
                }
                
        }}}
