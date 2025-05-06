pipeline {
    agent any

    stages {
        stage('Install Dependencies') {
            steps {
                echo 'Install dependencies...'
                bat 'pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running tests...'
                bat 'python -m pytest test_app.py'
            }
        }
    }

    post {
        success {
            // Mengirimkan notifikasi sukses ke Discord
            script {
                def webhookURL = 'https://discord.com/api/webhooks/1369318069889142877/K9EAGeKlveAGDFMB6o902L8onOUp5lIR2YEYn-UvY1DSzTuXJKZLbO3nD1PEWlXIET_J'
                def payload = '{"content": "✅ Build Success! Tes lulus semua."}'
                httpRequest httpMode: 'POST',
                            contentType: 'APPLICATION_JSON',
                            requestBody: payload,
                            url: webhookURL
            }
        }
        failure {
            // Mengirimkan notifikasi gagal ke Discord
            script {
                def webhookURL = 'https://discord.com/api/webhooks/1369318069889142877/K9EAGeKlveAGDFMB6o902L8onOUp5lIR2YEYn-UvY1DSzTuXJKZLbO3nD1PEWlXIET_J'
                def payload = '{"content": "❌ Build Gagal. Cek log untuk detail."}'
                httpRequest httpMode: 'POST',
                            contentType: 'APPLICATION_JSON',
                            requestBody: payload,
                            url: webhookURL
            }
        }
    }
}
