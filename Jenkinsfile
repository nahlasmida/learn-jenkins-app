pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                bat '''
                    docker run --rm -v "%WORKSPACE%:/src" -v npm-cache:/root/.npm node:18-alpine sh -c "mkdir /app && cd /src && tar cf - --exclude=node_modules --exclude=build --exclude=.git . | tar xf - -C /app && cd /app && chmod -R u+w . && ls -la && node --version && npm --version && npm ci && npm run build && ls -la && rm -rf /src/build && cp -r /app/build /src/build"
                '''
            }
        }

        stage('Test') {
            steps {
                bat '''
                    docker run --rm -v "%WORKSPACE%:/src" -v npm-cache:/root/.npm node:18-alpine sh -c "test -f /src/build/index.html && echo build/index.html exists && mkdir /app && cd /src && tar cf - --exclude=node_modules --exclude=build --exclude=.git . | tar xf - -C /app && cd /app && chmod -R u+w . && npm ci && CI=true npm test"
                '''
            }
        }
    }
}