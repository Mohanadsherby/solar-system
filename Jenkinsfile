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
                stage('Unit Testing') {
            steps {
                script {
                    // Install dependencies without auditing
                    sh 'npm test'
                }
            }
        }

    }
}    