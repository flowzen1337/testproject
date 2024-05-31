pipeline {
    agent any

    stages {
        stage('Checkstyle') {
            steps {
                script {
                    // Installiere Checkstyle falls es nicht vorhanden ist
                    sh 'if ! [ -x "$(command -v checkstyle)" ]; then echo "Checkstyle is not installed. Installing..."; sudo apt-get install -y checkstyle; fi'
                    // Führe Checkstyle aus
                    sh 'checkstyle -c /google_checks.xml src/main/java/**/*.java'
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
