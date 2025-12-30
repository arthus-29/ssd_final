pipeline {
    // This tells Jenkins to run this pipeline on any available agent/node
    agent any

    environment {
        // We define a virtual environment folder name
        VENV = 'venv'
    }

    stages {
        stage('1. Clone Repository') {
            steps {
                // This pulls the code from the GitHub repo you linked in the Jenkins Job
                checkout scm
            }
        }

        stage('2. Install Dependencies') {
            steps {
                script {
                    // Create a virtual environment and install flask/pytest
                    sh """
                    python3 -m venv $VENV
                    . $VENV/bin/activate
                    pip install --upgrade pip
                    pip install -r requirements.txt
                    """
                }
            }
        }

        stage('3. Run Unit Tests') {
            steps {
                script {
                    // Run pytest. If tests fail, the pipeline stops here.
                    sh """
                    . $VENV/bin/activate
                    export PYTHONPATH=$PYTHONPATH:.
                    pytest tests/test_app.py
                    """
                }
            }
        }

        stage('4. Build Application') {
            steps {
                echo "Packaging the application..."
                // In a real scenario, you might build a Docker image here.
                // For now, we'll create a zip file as a 'build artifact'.
                sh "zip -r flask_app.zip . -x '$VENV/*'"
            }
        }

        stage('5. Deploy App') {
            steps {
                // This is where you would move files to a production server
                echo "Deploying to Production..."
                sh """
                echo 'Deployment logic goes here (e.g., scp to server, docker run, or systemctl restart)'
                echo 'App is now live!'
                """
            }
        }
    }

    post {
        success {
            echo "Successfully built and deployed!"
        }
        failure {
            echo "Pipeline failed. Please check the logs."
        }
    }
}
