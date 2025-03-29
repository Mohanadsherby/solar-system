pipeline {
    agent any

    tools {
        nodejs 'nodejs-22-6-0'
    }
    environment {
        MONGO_URI = "mongodb+srv://supercluster.d83jj.mongodb.net/superData"

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
                 stage('Code Coverage') {
                            steps {
                                catchError(buildResult: 'SUCCESS', message: 'Oops! it will be fixed in future releases', stageResult: 'UNSTABLE') {
                                sh 'npm run coverage'
                                }

                                publishHTML([allowMissing: true, alwaysLinkToLastBuild: true, icon: '', keepAll: true, reportDir: 'coverage/lcov-report/', reportFiles: 'index.html', reportName: 'code coverage HTML Report', reportTitles: '', useWrapperFileDirectly: true])
                            }
                        }
                       stage('Unit Testing') {
                            options { retry(2) }
                             steps {
                                     sh 'npm test' 
                             }
                         }    
                    
                    }
                                
                }

            }

        }
 