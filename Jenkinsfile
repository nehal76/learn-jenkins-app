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
                 npm run build 
                 npx playwright test
                '''
            }
        }

        


        
    }
}

//  learn-jenkins-app\node_modules\.bin\serve -s build 
// npx serve -s build -l 3000 &