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
                    // Checkstyle Bericht veröffentlichen
                    checkstyle pattern: '**/target/checkstyle-result.xml'
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
