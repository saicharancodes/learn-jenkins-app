pipeline {
    agent any

    stages {
        
        stage ("with the docker") {
            agent {
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }}
            steps{
                    sh '''
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    '''
                }

                
        }

        stage ("test the app") {

            steps{

                sh'''
                test -f build/index.html && echo "Exists"
                npm test
                sss
                '''
            }

        
        
        }}}
