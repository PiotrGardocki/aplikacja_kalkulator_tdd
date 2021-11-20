pipeline {
    agent any
    parameters {
        gitParameter branchFilter: '.*', defaultValue: 'main', name: 'BRANCH', type: 'PT_BRANCH'
        choice(name: 'TEST_LEVEL', choices: ['smoke', 'regression', 'nightly'], description: 'Type od unit tests to run')
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
                    //buildName '#$BUILD_NUMBER'
                    buildDescription 'CI/CD Pipeline for TDD calculator'
                }
            }
        }
        stage('STATIC ANALYSE') {
            steps {
                script {
                    sh 'python3 -m pylint *.py --exit-zero'
                    //sh 'echo PYLINT'
                }
            }
        }
        stage('UNIT TESTS') {
            steps {
                script {
                    sh 'python3 -m pytest --html=report.html -m $TEST_LEVEL'
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

