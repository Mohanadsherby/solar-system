pipeline {
    agent any

    tools {
        nodejs 'nodejs-22-6-0'
    }

    stages {
        stage('Install Dependencies') {
            steps {
                script {
                    // Install dependencies without auditing
                    sh 'npm install --no-audit'
                }
            }
        }
        stage('DEpendency scanning '){
               parallel{     
                stage('NPM Dependency Audit') {
                    steps {
                        sh 'npm audit --audit-level=critical'
                        }
                    }
                stage('Owasp DEpendency Check') {
                    steps {
                        dependencyCheck additionalArguments: '''
                            --scan \'./\' 
                            --out \'./\'  
                            --format \'ALL\' 
                            --disableYarnAudit \
                            --prettyPrint''', odcInstallation:'dependency-check-owassp'
                       }             

                    }
                                
                }
            }

        }
    }    