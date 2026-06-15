pipeline {
    agent {
        label 'docker'
    }

    stages {
        stage('Build') {
            steps {
                sh 'make build'
            }
        }

        stage('Unit tests') {
            steps {
                sh 'make test-unit'
            }
            post {
                always {
                    junit 'results/unit_result.xml'
                    archiveArtifacts artifacts: 'results/unit_result.xml', allowEmptyArchive: true
                }
            }
        }

        stage('API tests') {
            steps {
                sh 'make test-api'
            }
            post {
                always {
                    junit 'results/api_result.xml'
                    archiveArtifacts artifacts: 'results/api_result.xml', allowEmptyArchive: true
                }
            }
        }

        stage('E2E tests') {
            steps {
                sh 'make test-e2e'
            }
            post {
                always {
                    junit 'results/cypress_result.xml'
                    archiveArtifacts artifacts: 'results/cypress_result.xml', allowEmptyArchive: true
                }
            }
        }
    }

    post {
        failure {
            mail to: 'reinaldo.diaz@comunidadunir.com',
                 subject: "Fallo en ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: """El pipeline ha finalizado con errores, por favor valida.

Trabajo: ${env.JOB_NAME}
Ejecucion: ${env.BUILD_NUMBER}
Resultado: ${currentBuild.currentResult}
URL: ${env.BUILD_URL}
"""
        }
    }
}
