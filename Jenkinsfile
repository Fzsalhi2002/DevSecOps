pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Cloner le dépôt
                git 'https://github.com/Fzsalhi2002/DevSecOps.git'
            }
        }
       
        stage('Test') {
            steps {
                // Exécuter les tests sous Windows
                bat 'mvn test'
            }
        }
        stage('Deploy') {
            steps {
                // Déploiement (peut être remplacé par votre stratégie de déploiement)
                bat 'echo "Déploiement de l\'application..."'
            }
        }
        stage('Secret Scanning') {
            steps {
                // Scanner les secrets avec Gitleaks
                bat 'gitleaks detect --source . --report-format json --report-path gitleaks-report.json'
            }
        }
        stage('Analyze Secrets Report') {
            steps {
                script {
                    // Vérifier si le rapport Gitleaks contient des secrets détectés
                    def report = readFile('gitleaks-report.json')
                    if (report.contains('leak')) {
                        error 'Secrets détectés ! Le build est arrêté.'
                    }
                }
            }
        }
        stage('SCA with Dependency-Check') {
            steps {
                echo 'Analyse de la composition des sources avec OWASP Dependency-Check...'
                // Exécuter l'analyse de la composition avec Dependency-Check en utilisant le chemin correct
                bat '"C:\\Users\\HP\\Downloads\\dependency-check-10.0.2-release\\dependency-check\\bin\\dependency-check.bat" --project "demo" --scan . --format HTML --out dependency-check-report.html --nvdApiKey 181c8fc5-2ddc-4d15-99bf-764fff8d50dc --disableAssembly'
            }
        }
    }

    post {
        always {
            // Archiver les fichiers importants à la fin
            archiveArtifacts artifacts: '**/target/*.jar', allowEmptyArchive: true
        }
        success {
            // Notifier en cas de succès
            echo 'Le build a réussi !'
        }
        failure {
            // Notifier en cas d'échec
            echo 'Le build a échoué.'
        }
    }
}
