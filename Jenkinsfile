pipeline {
    agent any

    stages {

        environment{
            netlify_project_id = '0a7a2af9-a8e7-471c-b97e-a977982be7cd'
            netlify_auth_token = credentials('netlify-token')
        }
     
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

        stage ("run them in parallel"){

            parallel{

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
                        image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                        reuseNode true
                    }}

                steps{

                    sh'''

                    npm install serve
                    node_modules/.bin/serve -s build &
                    sleep 10
                    npx playwright test


                '''
            }        }


        }}

        stage("deploy in netlify"){

            agent {
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }}
            steps{
                    sh '''
                        npm install netlify-cli@20.0.1
                        node_modules/.bin/netlify --version
                        echo 'deploying to production site id - $netlify_project_id '
                        node_modules/.bin/netlify status
                    '''
                }
        }
        
        
        
        
        }}


