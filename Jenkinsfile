pipeline {
    agent any

    stages {
        stage('without-docker') {
            steps {
                sh ''' echo "this is running without the docker locally code is from gitttu"
                touch withoutdocker.txt'''}}
        
        stage ("with the docker") {
            agent {
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }}
            steps{
                    echo "hello i'm running inside the dockerrr the code is from gitttu"
                    sh ''' npm --version
                    touch withdocker.txt
                    
                    '''
                }
                
        }}}
