pipeline {
    agent any

    environment{
            NETLIFY_SITE_ID = '0a7a2af9-a8e7-471c-b97e-a977982be7cd'
            NETLIFY_AUTH_TOKEN = credentials('netlify-token')
        }

    stages{
        
        stage('Docker image for node18 alphine'){

            steps{
                sh 'docker build -t node18-local .'
            }

        }
 
        stage("deploy in netlify -- TEST"){

            agent {
                docker{
                    image 'node18-local'
                    reuseNode true
                    
                }}
            steps{
                    sh '''
                        netlify --version
                        echo 'deploying to production site id - $NETLIFY_SITE_ID '
                        netlify status
                        netlify deploy --dir=build --json > deploy-output.json
                        node-jq -r '.deploy_url' deploy-output.json
                    '''
                    script { 

                        env.staging_site_id = sh (script:"node-jq -r '.deploy_url' deploy-output.json", returnStdout: true)
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
                    image 'node18-local'
                    reuseNode true
                }}
            steps{
                    sh '''
                        npm install netlify-cli@20.0.1
                        netlify --version
                        echo 'deploying to production site id - $NETLIFY_SITE_ID '
                        netlify status
                        netlify deploy --dir=build --prod
                    '''
                }
        }
        
        
        }}


