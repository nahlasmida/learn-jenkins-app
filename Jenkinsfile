pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                bat '''
                    docker run --rm -v "%WORKSPACE%:/app" -w /app node:18-alpine sh -c "ls -la && node --version && npm --version && npm ci && npm run build && ls -la"
                '''
            }
        }
    }
}