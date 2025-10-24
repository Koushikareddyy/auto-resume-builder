
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Pull code from GitHub
                git 'https://github.com/Koushikareddyy/auto-resume-builder.git'
            }
        }

        stage('Build Resume') {
            steps {
                // Convert markdown to PDF
                sh '''
                    echo "Generating resume PDF..."
                    pandoc resume.md -o resume.pdf
                '''
            }
        }

        stage('Archive PDF') {
            steps {
                // Save PDF to Jenkins artifacts
                archiveArtifacts artifacts: 'resume.pdf', fingerprint: true
            }
        }
    }

    post {
        success {
            echo '✅ Build completed successfully — resume.pdf generated!'
        }
        failure {
            echo '❌ Build failed. Check console output for errors.'
        }
    }
}
