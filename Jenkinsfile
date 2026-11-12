pipeline {
    agent any
    environment {
        NETLIFY_SITE_ID = '6cfe2b8d-1853-47aa-8721-630fc0974b8e'
    }
    stages {
        stage('Build') {
            steps {
                bat '''
                    docker run --rm -v "%WORKSPACE%:/src" -v npm-cache:/root/.npm node:18-alpine sh -c "mkdir /app && cd /src && tar cf - --exclude=node_modules --exclude=build --exclude=.git . | tar xf - -C /app && cd /app && chmod -R u+w . && npm ci && npm run build && rm -rf /src/build && cp -r /app/build /src/build"
                && echo Deploying to production. Site ID: $NETLIFY_SITE_ID'''
            }
        }

        stage('Test') {
            steps {
                bat '''
                    docker run --rm -v "%WORKSPACE%:/src" -v npm-cache:/root/.npm node:18-alpine sh -c "test -f /src/build/index.html && echo build/index.html exists && mkdir /app && cd /src && tar cf - --exclude=node_modules --exclude=build --exclude=.git . | tar xf - -C /app && cd /app && chmod -R u+w . && npm ci && export CI=true JEST_JUNIT_OUTPUT_DIR=/app/test-results JEST_JUNIT_OUTPUT_NAME=junit.xml && npm test -- --reporters=default --reporters=jest-junit; rc=$?; mkdir -p /src/test-results; cp -r /app/test-results/. /src/test-results/; exit $rc"
                '''
            }
        }

        stage('Deploy') {
            steps {
                bat '''
                    docker run --rm -v npm-cache:/root/.npm node:18-alpine sh -c "npm install -g netlify-cli && netlify --version"
                '''
            }
        }
    }

    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}