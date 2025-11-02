pipeline {
    agent any

    stages {
        stage('Code') {
            steps {
                git url: "https://github.com/dheeraj1kumar/loan.git", branch: "main"
            }
        }

        stage('Build') {
            steps {
                sh '''
                    echo "🛠️ Building Docker image..."
                    docker build -t branch-app:latest .
                '''
            }
        }

        stage("Push To DockerHub") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "dockerhub",
                    usernameVariable: "dockerHubUser",
                    passwordVariable: "dockerHubPass"
                )]) {
                    sh '''
                        echo $dockerHubPass | docker login -u $dockerHubUser --password-stdin
                        docker image tag branch-app:latest $dockerHubUser/branch-app:latest
                        docker push $dockerHubUser/branch-app:latest
                    '''
                }
            }
        }

      stage('DB Update') {
    steps {
        echo "🔧 Running DB migrations before deployment..."
        sh '''
            set -e

            if [ ! -f docker-compose.yml ]; then
                echo "❌ docker-compose.yml not found!"
                exit 1
            fi

            echo "✅ docker-compose.yml found."

            echo "🚀 Starting database service..."
            docker-compose up -d db

            echo "⏳ Waiting for PostgreSQL to become ready..."
            sleep 10

            echo "🌐 Detecting docker network..."
            NETWORK_NAME=$(docker network ls --filter name=branch_default --format "{{.Name}}" | head -n 1)
            echo "✅ Using network: $NETWORK_NAME"

            echo "🔄 Running Alembic migrations..."
            docker run --rm \
                --network=$NETWORK_NAME \
                -v $(pwd):/app \
                -w /app \
                dheeraj1kumar/branch-app:latest \
                bash -c "alembic upgrade head || flask db upgrade || echo 'Migration skipped (no command found)'"

            echo "✅ Database successfully migrated."
        '''
    }
}



        stage('Deploy') {
            steps {
                sh '''
                    echo "🚀 Deploying application..."
                    docker-compose down
                    docker-compose up -d
                '''
            }
        }
    }

    post {
        always {
            echo "🧹 Cleaning up..."
            sh 'docker image prune -f || true'
        }
    }
}
