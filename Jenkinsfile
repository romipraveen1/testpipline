pipeline {
    agent {
        docker {
            image 'node:lts-buster-slim'
            args '-p 3000:3000'
        }
    }
    stages {
    
        stage('Install Packages') { 
            steps {
                sh 'npm install --legacy-peer-deps' 
            }
        }

         stage('Test') {
            steps {
                sh 'npm test'
            }
        }

        stage('Create Build Artifacts') {
          steps {
            sh 'npm run build'
          }
        }
       

        stage('Production') {
            steps {
                withAWS(region:'ap-southeast-1',credentials:'CREDENTIALS_FROM_JENKINS_SETUP') {
                s3Delete(bucket: 'th-test-deploy', path:'**/*')
                s3Upload(bucket: 'th-test-deploy', workingDir:'build', includePathPattern:'**/*');
                }
            }
        }
    }

}