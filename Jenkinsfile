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
                    # Ensure Docker Compose file exists
                    if [ -f docker-compose.yml ]; then
                        echo "✅ docker-compose.yml found."
                    else
                        echo "❌ docker-compose.yml not found. Exiting..."
                        exit 1
                    fi

                    # Start DB temporarily for migration
                    docker-compose up -d db

                    echo "⏳ Waiting for PostgreSQL to become ready..."
                    sleep 10

                    # Run migration commands inside the API container
                    docker run --rm \
                        --network=$(basename $(pwd))_default \
                        -v $(pwd):/app \
                        -w /app \
                        branch-app:latest \
                        sh -c "flask db upgrade"

                    echo "✅ Database successfully migrated."
                '''
            }
        }

        stage('Test') {
            steps {
                echo "🧪 Running tests..."
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
