// This pipeline automates the conversion of resume.md to resume.pdf
pipeline {
    // 1. Agent: Specifies where the job runs. 'any' means any available agent.
    //    ***CRITICAL: This agent must have Pandoc and TeX Live installed.***
    agent any 

    options {
        // Discard old builds to save disk space, keeping only the last 10 builds.
        // FIX: Corrected parameter names to 'daysToKeepStr' and 'numToKeepStr' 
        // to comply with Jenkins declarative pipeline syntax.
        buildDiscarder(logRotator(daysToKeepStr: '30', numToKeepStr: '10'))
    }

    stages {
        stage('Checkout Source Code') {
            steps {
                echo "Pulling latest resume files from GitHub..."
                // This command pulls your repository (including 'resume.md') onto the agent.
                checkout scm
            }
        }

        stage('Build PDF Artifact') {
            steps {
                echo "Converting Markdown to PDF using Pandoc..."
                // 2. The Core Conversion Command:
                //    - pandoc resume.md -o resume.pdf : Takes input 'resume.md' and outputs 'resume.pdf'.
                //    - --pdf-engine=xelatex           : Uses the TeX engine for high-quality, professional PDF formatting.
                //    - -V geometry:margin=1in         : Sets a clean, standard 1-inch margin.
                sh 'pandoc resume.md -o resume.pdf --from markdown --pdf-engine=xelatex -V geometry:margin=1in'
            }
        }

        stage('Archive Artifact') {
            steps {
                // 3. Artifact Management: Makes the generated PDF downloadable from the Jenkins build page.
                echo "Archiving resume.pdf for download."
                archiveArtifacts artifacts: 'resume.pdf', fingerprint: true
            }
        }
    }
    
    // Notifications and cleanup after all stages are complete
    post {
        success {
            echo "✅ Build SUCCESS! The latest resume.pdf is ready and archived."
        }
        failure {
            echo "❌ Build FAILED! Check agent logs for Pandoc errors or syntax issues in resume.md."
        }
    }
}
