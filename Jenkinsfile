pipeline {
    agent any
    parameters {
        gitParameter branchFilter: 'origin/(.*)', defaultValue: 'main', name: 'BRANCH', type: 'PT_BRANCH'
    }

    stages {
        stage('CHECKOUT') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '$BRANCH']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/PiotrGardocki/aplikacja_kalkulator_tdd']]])
            }
        }
        stage('SET DESCRIPTION') {
            steps {
                script {
                    #buildName '#$BUILD_NUMBER'
                    buildDescription 'CI/CD Pipeline for TDD calculator'
                }
            }
        }
        stage('STATIC ANALYSE') {
            steps {
                script {
                    sh 'python3 -m pylint'
                }
            }
        }
        stage('UNIT TESTS') {
            steps {
                script {
                    sh 'python3 -m pytest --html=report.html'
                }
            }
        }
        stage('ARCHIVE') {
            steps {
                script {
                    archiveArtifacts artifacts: 'report.html, assets/', followSymlinks: false
                }
            }
        }
        stage('CLEANUP') {
            steps {
                script {
                    cleanWs()
                }
            }
        }
    }
}

