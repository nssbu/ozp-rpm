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
                copyArtifacts projectName: 'ozp-backend', filter: 'backend.tar.gz'
                copyArtifacts projectName: 'ozp-center', filter: 'center.tar.gz'
                copyArtifacts projectName: 'ozp-help', filter: 'help.tar.gz'
                copyArtifacts projectName: 'ozp-hud', filter: 'hud.tar.gz'
                copyArtifacts projectName: 'ozp-iwc', filter: 'iwc.tar.gz'
                copyArtifacts projectName: 'ozp-webtop', filter: 'webtop.tar.gz'
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
        stage('Copy the OZP RPM to the Home Directory') {
            steps {
                sh 'cp /var/jenkins/workspace/ozp-rpm/target/rpm/ozp/RPMS/noarch/ozp-*.rpm ~'
            }
        }
    }
}