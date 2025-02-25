pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                // Checkout the branch from the Git repository
                git branch: 'master', url: 'https://github.com/Nischalpradhan13/simple-node-js-react-npm-app.git'
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
                sshPublisher(publishers: [sshPublisherDesc(configName: 'Alma_Linux', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '''cd /root/nodeapp
npm install
npm run start
sudo systemctl restart nginx''', flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '\'build/**, package.json, package-lock.json\'')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            }
        }
    }
}
