pipeline {
    agent any

    tools {
        nodejs 'nodejs-22-6-0'
    }
    environment {
        MONGO_URI = "mongodb+srv://cluster0.b8k0ocr.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0"

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

                withCredentials([usernamePassword(credentialsId: 'mongo-db-credintials', passwordVariable: 'MONGO_PASSWORD', usernameVariable: 'MONGO_USERNAME')]) {
                    // some block
                }
                    sh 'npm test'
            }
        }
    }

}
    