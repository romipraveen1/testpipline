pipeline {
    agent {
        docker {
            image 'node:lts-buster-slim'
            args '-p 3000:3000'
        }
    }
    stages {
         stage('Test') {
            steps {
                sh 'npm test'
            }
        }
        stage('Build') { 
            steps {
                sh 'npm install --legacy-peer-deps' 
            }
        }
       

        stage('Production') {
            steps {
                withAWS(region:'Global',credentials:'CREDENTIALS_FROM_JENKINS_SETUP') {
                s3Delete(bucket: 'th-test-deploy', path:'**/*')
                s3Upload(bucket: 'th-test-deploy', workingDir:'build', includePathPattern:'**/*');
                }
            }
        }
    }

}