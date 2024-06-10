pipeline {
    agent any

    environment {
        OTEL_SERVICE_NAME = 'jenkins'
        OTEL_EXPORTER_OTLP_ENDPOINT = 'http://otel-collector:4317'
    }
    
    tools {
        // Nimmt an, dass du bereits Maven installiert hast
        maven 'Maven 3.6.3' // Definiere deine Maven-Version hier
    }

    stages {
        stage('Checkstyle') {
            steps {
                script {
                    recordStartTime('Checkstyle')
                    def mvnCmd = 'mvn checkstyle:checkstyle'
                    def mvnOutput = sh(script: mvnCmd, returnStdout: true).trim()
                    echo mvnOutput
                    // Fail the build if Checkstyle output contains 'ERROR'
                    if (mvnOutput.contains('ERROR')) {
                        echo "Checkstyle found errors. Failing the build."
                        error('Checkstyle found errors. Failing the build.')
                    }
                    // Fail the build if Checkstyle output contains 'WARN' and 'warnError' option is enabled
                    if (mvnOutput.contains('[WARN]')) {
                        echo "Checkstyle found warnings. Marking the build as unstable."
                        unstable('Checkstyle found warnings. Marking the build as unstable.')
                    }
                }
            }
            post {
                always {
                    recordEndTime('Checkstyle')
                    archiveArtifacts artifacts: '**/target/checkstyle-result.xml', allowEmptyArchive: true
                }
            }
        }
        stage('Step 2') {
            steps {
                script {
                    recordStartTime('Step 2')
                    echo 'step 2'
                }
            }
            post {
                always {
                    recordEndTime('Step 2')
                }
            }
        }
        stage('Step 3') {
            steps {
                script {
                    recordStartTime('Step 3')
                    echo 'step 3'
                }
            }
            post {
                always {
                    recordEndTime('Step 3')
                }
            }
        }
    }

    post {
        always {
            script {
                currentBuild.result = currentBuild.result ?: 'SUCCESS'
            }
        }
        failure {
            script {
                emailext(
                    subject: "Jenkins Build Failed: ${env.JOB_NAME} [${env.BUILD_NUMBER}]",
                    body: """<p>Build ${env.BUILD_NUMBER} failed in stage: ${env.STAGE_NAME}</p>
                             <p>Job: ${env.JOB_NAME}</p>
                             <p>Build URL: <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                             <p>Stage: ${env.STAGE_NAME}</p>
                             <p>Result: ${currentBuild.result}</p>""",
                    to: 'your-email@example.com'
                )
            }
        }
    }
}

def recordStartTime(stageName) {
    // Metrik für Startzeit der Stage erfassen und an den Collector senden
    def stageNameValue = "${stageName}"
    sh "echo 'stage_start_time_seconds{name=\"$stageNameValue\"} $(date +%s.%N)' | curl -X POST -H 'Content-Type: text/plain' --data-binary @- ${env.OTEL_EXPORTER_OTLP_ENDPOINT}/metrics"
}

def recordEndTime(stageName) {
    // Metrik für Endzeit der Stage erfassen und an den Collector senden
    def stageNameValue = "${stageName}"
    sh "echo 'stage_end_time_seconds{name=\"${stageNameValue}\"} $(date +%s.%N)' | curl -X POST -H 'Content-Type: text/plain' --data-binary @- ${env.OTEL_EXPORTER_OTLP_ENDPOINT}/metrics"
}
