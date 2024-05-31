pipeline {
    agent any

    tools {
        // Nimmt an, dass du bereits Maven installiert hast
        maven 'Maven 3.6.3' // Definiere deine Maven-Version hier
    }

    stages {
        stage('Checkstyle') {
            steps {
                script {
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
                    archiveArtifacts artifacts: '**/target/checkstyle-result.xml', allowEmptyArchive: true
                }
            }
        }
        stage('Step 2') {
            steps {
                echo 'step 2'
            }
        }
        stage('Step 3') {
            steps {
                echo 'step 3'
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
