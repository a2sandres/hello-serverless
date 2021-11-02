pipeline {
    agent any
    stages {
        stage('build sin test') {
            steps {
                nodejs(nodeJSInstallationName: 'nodejs') {
                    sh 'npm install'
                    sh 'npm rebuild'
                    sh 'npm run build --skip-test --if-present'
                    // stash name: "ws", invludes: "**"
                }
            }    
        }

        stage('unitTest') {
            steps {
                	//untash "ws"
                    nodejs(nodeJSInstallationNamwe: 'nodejs') {
                        sh 'npm run test:coverage && cp coverage/lcov.info lcov.info || echo "Code coverage failed"'
                        archiveArtifacts(artifacts: 'coverage/**', onlyIFSuccessful:true)
                    }
            }
        }

        stage('deploy') {
            steps{
                nodejs(nodeJSInstallationName: 'nodejs') {
                    withAWS(credentials: 'aws-credentials') {
                        sh 'serverless deploy'
                    }
                }
            }
        }
        
    }
}