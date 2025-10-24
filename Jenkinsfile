pipeline {
    agent any

    environment {
        PANDOC_PATH = '/opt/homebrew/bin/pandoc'
        TINYTEX_PATH = '/Users/koushikareddysingasani/Library/TinyTeX/bin/x86_64-darwin'
        PATH = "${env.PATH}:${TINYTEX_PATH}"
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
                echo '🛠️ Building resume using Pandoc + TinyTeX...'
                sh '''
                    echo "Current workspace: $(pwd)"
                    echo "Files before build:"
                    ls -l
                    echo "Generating resume PDF with XeLaTeX engine..."
                    ${PANDOC_PATH} resume.md -o resume.pdf --pdf-engine=xelatex \
                    --metadata title="Koushika Reddy Resume" \
                    -V geometry:margin=1in -V mainfont="Helvetica" -V fontsize=11pt -V colorlinks=true
                    echo "Files after build:"
                    ls -l
                '''
            }
        }

        stage('Archive PDF') {
            steps {
                echo '📦 Checking and archiving generated PDF...'
                sh 'ls -l resume.pdf || echo "resume.pdf not found!"'
                archiveArtifacts artifacts: 'resume.pdf', onlyIfSuccessful: true, fingerprint: true
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
