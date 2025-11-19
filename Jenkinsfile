pipeline {
    agent any
    
    options {
        ansiColor('xterm')
    }

    stages {
        stage('build') {
            agent {
                docker {
                    image 'node:22-alpine'
                    reuseNode true
                }
            }
            steps {
                sh 'npm ci'
                sh 'npm run build'
                echo 'Running tests on same stage'
                sh 'npx vitest run --reporter=verbose'
            }
        }

        stage('test') {
            parallel {
                stage('unit tests') {
                    agent {
                        docker {
                            image 'node:25.2.1-trixie'
                            reuseNode true
                        }
                    }
                    steps {
                        // Unit tests with Vitest
                        // sh 'npx vitest run --reporter=verbose'
                        echo 'Mock test was successful!'
                    }
                }

                stage('integration tests') {
                    agent {
                        docker {
                            image 'mcr.microsoft.com/playwright:v1.54.2-jammy'
                            reuseNode true
                        }
                    }
                    steps {
                        sh 'npx playwright test'
                    }
                }

            }
        }

        stage('deploy') {
            agent {
                docker {
                    image 'alpine'
                }
            }
            steps {
                // Mock deployment which does nothing
                echo 'Mock deployment was successful!'
            }
        }

        stage('integration tests') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.54.2-jammy'
                    reuseNode true
                }
            }
            steps {
                sh 'npx playwright test'
            }
            post {
                always {
                    publishHTML([
                        allowMissing: false,
                        alwaysLinkToLastBuild: true,
                        keepAll: true,
                        reportDir: 'reports-e2e/html',
                        reportFiles: 'index.html',
                        reportName: 'Playwright Test Report - DanyTheDev',
                        reportTitles: '',
                        useWrapperFileDirectly: true
                    ])
                    junit stdioRetention: 'ALL', testResults: 'reports-e2e/junit.xml'
                }
            }
        }

        
        stage('e2e') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.54.2-jammy'
                    reuseNode true
                }
            }
            environment {
                // Using localhost as the site no longer exists
                // E2E_BASE_URL = 'https://spanish-cards.netlify.app/'
                E2E_BASE_URL = 'http://localhost:8080'
            }
            steps {
                sh 'npx playwright test'
            }
        }
    }
}
