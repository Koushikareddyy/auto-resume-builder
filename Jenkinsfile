pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo '📥 Checking out code from GitHub...'
                git branch: 'main', url: 'https://github.com/Koushikareddyy/auto-resume-builder.git'
            }
        }

        stage('Build Resume') {
            steps {
                echo '🛠️ Building resume using Pandoc + TinyTeX...'
                sh '''
                    echo "Using local Eisvogel template..."
                    /opt/homebrew/bin/pandoc resume.md -o resume.pdf \
                        --from markdown \
                        --template templates/eisvogel.tex \
                        --pdf-engine=xelatex \
                        -V geometry:margin=1in \
                        -V mainfont=Helvetica \
                        -V fontsize=11pt \
                        -V colorlinks=true
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
            echo '❌ Build failed. Check logs.'
        }
    }
}
