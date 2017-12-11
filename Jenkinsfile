pipeline {
    agent {
        label 'KZ01_TI-141_OZP_CentOS'
    }
    stages {
        stage('Clear the Workspace') {
            steps {
                sh 'rm -rf *'
            }
        }
        stage('Checkout Repo') {
            steps {
                git url: 'http://github.com/mark-betters-ozp-forks/ozp-rpm.git', branch: 'master'
            }
        }
        stage('Unarchive OZP Tarballs') {
            steps {
                sh '''
                  mkdir sources
                  mkdir -p sources/backend
                  mkdir -p sources/frontend
                '''
                copyArtifacts projectName: 'ozp-backend', filter: 'backend.tar.gz', target: 'sources/backend'
                copyArtifacts projectName: 'ozp-center', filter: 'center.tar.gz', target: 'sources/frontend'
                copyArtifacts projectName: 'ozp-help', filter: 'help.tar.gz', target: 'sources/frontend'
                copyArtifacts projectName: 'ozp-hud', filter: 'hud.tar.gz', target: 'sources/frontend'
                copyArtifacts projectName: 'ozp-iwc', filter: 'iwc.tar.gz', target: 'sources/frontend'
                copyArtifacts projectName: 'ozp-webtop', filter: 'webtop.tar.gz', target: 'sources/frontend'
            }
        }
        stage('Build the RPM') {
            steps {
                //TODO: Set the BUILD_NUMBER environment variable to a proper value before running the next line.
                sh 'mvn rpm:rpm'
            }
        }
        stage('Archive the RPM') {
            steps {
                archiveArtifacts artifacts: '**/*.rpm'
            }
        }
        stage('Copy the RPM into the OZP Package in the Home Directory') {
            steps {
                sh '''
                  rm ~/ozppkg/ozp-*.rpm
                  cp /var/jenkins/workspace/ozp-rpm/target/rpm/ozp/RPMS/noarch/ozp-*.rpm ~/ozppkg
                '''
            }
        }
    }
}