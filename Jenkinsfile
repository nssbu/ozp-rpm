pipeline {
    agent {
        label 'KZ01_TI-141_OZP_CentOS'
    }
    stages {
        stage('Checkout Repo') {
            steps {
                git url: 'http://github.com/mark-betters-ozp-forks/ozp-rpm.git', branch: 'master'
            }
        }
        stage('Unarchive OZP Tarballs') {
            steps {
                sh '''
                  rm -rf sources
                  mkdir sources
                  mkdir -p sources/backend
                  mkdir -p sources/frontend
                '''
                unarchive mapping: ['../ozp-backend/backend.tar.gz': 'sources/backend/backend.tar.gz']
                unarchive mapping: ['../ozp-center/center.tar.gz': 'sources/frontend/center.tar.gz']
                unarchive mapping: ['../ozp-help/help.tar.gz': 'sources/frontend/help.tar.gz']
                unarchive mapping: ['../ozp-hud/hud.tar.gz': 'sources/frontend/hud.tar.gz']
                unarchive mapping: ['../ozp-iwc/iwc.tar.gz': 'sources/frontend/iwc.tar.gz']
                unarchive mapping: ['../ozp-webtop/webtop.tar.gz': 'sources/frontend/webtop.tar.gz']
            }
        }
        stage('Build the RPMs') {
            steps {
                sh 'mvn rpm:rpm'
            }
        }
        stage('Archive the RPMs') {
            steps {
                archiveArtifacts artifacts: '**/*.rpm'
            }
        }
    }
}