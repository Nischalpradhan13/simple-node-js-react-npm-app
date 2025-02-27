pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                // Checkout the branch from the Git repository
                git branch: 'test', url: 'https://github.com/Nischalpradhan13/simple-node-js-react-npm-app.git'
            }
        }
        stage('Setup') {
            steps {
                sh 'node -v'
                sh 'npm -v'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh 'npm test'
            }
        }
        stage('Build Stage') {
            steps {
                sh 'npm run build'
            }
        }
        stage('Deploy to Alma Linux') {
             steps {
                sshPublisher(publishers: [sshPublisherDesc(configName: 'Alma_linux_withoutpath', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'rm -rf testapp/*;', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: ''), sshTransfer(cleanRemote: false, excludes: '', execCommand: '''cd testapp
npm install
npm run build
pm2 start npm --name "testapp" -- start''', execTimeout: 360000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '/root/testapp', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '**/**')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
        
             }
        }
    }
}
