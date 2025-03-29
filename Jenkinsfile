pipeline {
    agent any

    tools {
        nodejs 'nodejs-22-6-0'
    }

    stages {

        stage('git checkout') {
            steps {
                    checkout scmGit(branches: [[name: '*/feature1']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Mohanadsherby/solar-system.git']])
                                }
        }    

        stage('Install Dependencies') {
            steps {
                script {
                    // Install dependencies without auditing
                    sh 'npm install --no-audit'
                }
            }
        }

        stage('NPM Dependency Audit') {
            steps {
                sh 'npm audit --audit-level=critical'
                }
            }
        stage('Owasp DEpendency Check') {
            steps {
                    dependencyCheck additionalArguments: '''
                            --scan "./"
                            --disableYarnAudit
                            --format ALL
                            --prettyPrint
                        ''', odcInstallation: 'dependency-check-owassp'
                }
            }

        }
    }    