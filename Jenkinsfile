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
    }
}
