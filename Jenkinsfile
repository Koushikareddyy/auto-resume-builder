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
        echo "Using local Eisvogel template..."
        /opt/homebrew/bin/pandoc resume.md -o resume.pdf --from markdown --template templates/eisvogel.tex --pdf-engine=xelatex -V geometry:margin=1in -V mainfont=Helvetica -V fontsize=11pt -V colorlinks=true
        '''
    }
}


        stage('Archive PDF') {
            steps {
                echo '📦 Archiving resume PDF...'
                archiveArtifacts artifacts: 'resume.pdf', onlyIfSuccessful: true
            }
        }
    }

    post {
        success {
            echo '✅ Resume built successfully and archived!'
        }
        failure {
            echo '❌ Build failed. Check logs.'
        }
    }
}
