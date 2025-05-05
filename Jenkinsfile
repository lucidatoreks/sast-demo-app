pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                // No need for explicit git checkout as Jenkins will do this automatically
                // when using "Pipeline script from SCM" option
                echo 'Using code from SCM checkout'
            }
        }
        stage('Install Dependencies') {
            steps {
                // Create a virtual environment to avoid system-level pip issues
                sh '''
                    python3 -m venv venv || python -m venv venv
                    . venv/bin/activate
                    pip install bandit
                '''
            }
        }
        stage('SAST Analysis') {
            steps {
                sh '''
                    . venv/bin/activate
                    bandit -f xml -o bandit-output.xml -r . || true
                '''
                recordIssues tools: [bandit(pattern: 'bandit-output.xml')]
            }
        }
    }
}
