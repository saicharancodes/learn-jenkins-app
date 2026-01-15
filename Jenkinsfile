pipeline {
    agent any

    stages {
       /* 
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

                
        }*/

        stage ("test the app") {

            agent {
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }}

            steps{

                sh'''
                test -f build/index.html && echo "Exists"
                npm test
            
                '''
            }}

        stage('E2E Testing'){

            agent {
                docker{
                    image 'mcr.microsoft.com/playwright:v1.57.0-noble'
                    reuseNode true
                }}

            steps{

                sh'''

                npm install serve
                nodemodules/.bin/serve -s build &
                sleep 10
                npx playwrite test


                '''
            }

        }
        
        }}
