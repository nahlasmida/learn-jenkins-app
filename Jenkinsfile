pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                bat '''
                    docker run --rm -v "%WORKSPACE%:/src" node:18-alpine sh -c "mkdir /app && cd /src && tar cf - --exclude=node_modules --exclude=build --exclude=.git . | tar xf - -C /app && cd /app && chmod -R u+w . && npm ci && npm run build && rm -rf /src/build && cp -r /app/build /src/build"
                '''
            }
        }
    }
}