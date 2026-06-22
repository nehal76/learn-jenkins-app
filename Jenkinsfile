// pipeline {
//     agent any

//     stages {
//         stage('Hello') {
//             steps {
//                 echo 'Hello, Jenkins!'
//             }
//         }
//     }
// }

pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = "47f7d661-8b65-4946-b006-9293b12b7bec"
        NETLIFY_AUTH_TOKEN = credentials('netlify-tocken')
    }

    stages {

        stage('Test'){
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }

            steps{
                sh '''

                ls -la
                node --version
                npm --version
                npm ci
                npm run test
                ls -la
                '''
            }
        }
    
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                    //let's go
                }
            }
            steps {
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                    
                '''
            }
        }

        stage('E2E') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.59.1-noble'
                    reuseNode true
                }
            }
            steps {
                sh '''
                 npm ci
                 npx playwright test
                '''
            }
        }

        // stage ('Deploy'){
        //     agent{
        //         docker {
        //             image 'node:18-alpine'
        //             reuseNode true
        //         }
        //     }

        //     steps {
        //         sh '''
        //         npm install -g netlify-cli
        //         netlify --version
        //         echo "Deploying  to production. Site ID: $NETLIFY_SITE_ID"
        //         netlify status
        //         '''
        //     }
        // }

        stage ('Deploy to ec2'){
            agent{
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }

           
            steps {
                sh '''
                apk add --no-cache openssh-client

                echo "Copying build files to EC2..."
                scp -o StrictHostKeyChecking=no -i react-ami.pem  -r build/* ubuntu@3.107.38.112:/home/ubuntu/
                 '''
            }

        }

    }
}

//  learn-jenkins-app\node_modules\.bin\serve -s build 
// npx serve -s build -l 3000 &

// netlify deploy --dir=build --prod

// http://13.210.249.135:8080/multibranch-webhook-trigger/invoke?token=my-token

// http://13.210.249.135:8080/github-webhook/