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
                    // Checkstyle über Maven ausführen
                    sh 'mvn checkstyle:checkstyle'
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
