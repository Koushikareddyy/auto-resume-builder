pipeline {
    agent any

    environment {
        // Full path to Pandoc — adjust if your `which pandoc` gives a different path
        PANDOC_PATH = '/opt/homebrew/bin/pandoc'
    }

    stages {
        stage('Checkout') {
            steps {
                echo '📥 Checking out code from GitHub...'
                git 'https://github.com/Koushikareddyy/auto-resume-builder.git'
            }
        }

        stage('Build Resume') {
            steps {
                echo '🛠️ Building resume using Pandoc...'
                sh '''
                    echo "Generating resume PDF..."
                    ${PANDOC_PATH} resume.md -o resume.pdf
                '''
            }
        }

        stage('Archive PDF') {
            steps {
                echo '📦 Archiving generated PDF...'
                archiveArtifacts artifacts: 'resume.pdf', fingerprint: true
            }
        }
    }

    post {
        success {
            echo '✅ Build completed successfully — resume.pdf generated and archived!'
        }
        failure {
            echo '❌ Build failed. Check console output for errors.'
        }
    }
}
