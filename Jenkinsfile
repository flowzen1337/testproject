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
}
