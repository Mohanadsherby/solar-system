pipeline {
    agent any

    tools {
        nodejs 'nodejs-22-6-0'
    }
    environment {
        MONGO_URI = "mongodb+srv://cluster0.b8k0ocr.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0"
        SONARSONAR_SCANNER_HOME = tool 'SONAR_QUBE';

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
                    stage('OWASP Dependency Check') {
                                steps {
                                    dependencyCheck additionalArguments: '''  --scan \\\'./\\\' 
                                    --out \\\'./\\\'  
                                    --format \\\'ALL\\\' 
                                    --disableYarnAudit \\''', odcInstallation: 'dependency-check-owassp'

                                dependencyCheckPublisher pattern: 'dependency-check-report.xml', unstableTotalCritical: 1 

                    }
                }

            }

        }

        stage('Code Coverage') {
            steps {
                catchError(buildResult: 'SUCCESS', message: 'Oops! it will be fixed in future releases', stageResult: 'UNSTABLE') {
                    sh 'npm run coverage'
                }
                 publishHTML([allowMissing: false, alwaysLinkToLastBuild: true, icon: '', keepAll: true, reportDir: 'coverage/lcov-report/', reportFiles: 'index.html', reportName: 'Code Coverage HTML Report', useWrapperFileDirectly: true])


            }
        }

    }
}    

    //     stage('Unit Testing') {
    // steps {

    //     withCredentials([usernamePassword(credentialsId: 'mongo-db-credintials', passwordVariable: 'MONGO_PASSWORD', usernameVariable: 'MONGO_USERNAME')]) {
    //         // some block
    //                         }
    //                             sh 'npm test'
    //                     }
    //                 }
    //             }
    


 