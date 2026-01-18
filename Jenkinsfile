pipeline {
    agent any

    environment{
            NETLIFY_SITE_ID = '0a7a2af9-a8e7-471c-b97e-a977982be7cd'
            NETLIFY_AUTH_TOKEN = credentials('netlify-token')
        }

    stages{
 
        stage("deploy in netlify -- TEST"){

            agent {
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                    
                }}
            steps{
                    sh '''
                        rm -rf node_modules
                        npm install netlify-cli@20.0.1 node-jq
                        node_modules/.bin/netlify --version
                        echo 'deploying to production site id - $NETLIFY_SITE_ID '
                        node_modules/.bin/netlify status
                        node_modules/.bin/netlify deploy --dir=build --json > deploy-output.json
                        node_modules/.bin/node-jq -r '.deploy_url' deploy-output.json
                    '''
                    script { 

                        env.staging_site_id = sh (script:"node_modules/.bin/node-jq -r '.deploy_url' deploy-output.json", returnStdout: true)
                    }
                }
        }

        stage('E2E Testing'){

                agent {
                    docker{
                        image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                        reuseNode true
                    }}

                steps{

                    sh"""
                    echo "here is the site url ${env.staging_site_id}"


                """
            }        }

        stage('approval') {
            steps {
                timeout(time: 1, unit: 'MINUTES') {
                input message: 'yes', ok: 'yes i am sure'
                        }

            }
        }

        stage("deploy in netlify -- PROD"){

            agent {
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }}
            steps{
                    sh '''
                        npm install netlify-cli@20.0.1
                        node_modules/.bin/netlify --version
                        echo 'deploying to production site id - $NETLIFY_SITE_ID '
                        node_modules/.bin/netlify status
                        node_modules/.bin/netlify deploy --dir=build --prod
                    '''
                }
        }
        
        
        }}


