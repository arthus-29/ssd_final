pipeline {
    agent any

    environment {
        // Windows uses a different path style
        VENV = 'venv'
    }

    stages {
        stage('1. Clone Repository') {
            steps {
                checkout scm
            }
        }

        stage('2. Install Dependencies') {
            steps {
                // 'bat' is for Windows Command Prompt
                bat """
                python -m venv %VENV%
                call %VENV%\\Scripts\\activate
                pip install -r requirements.txt
                """
            }
        }

        stage('3. Run Unit Tests') {
            steps {
                bat """
                call %VENV%\\Scripts\\activate
                set PYTHONPATH=.
                pytest Test/test.py
                """
            }
        }

        stage('4. Build Application') {
            steps {
                echo "Packaging Application for Windows..."
                // Using powershell to create a zip on Windows
                powershell "Compress-Archive -Path * -DestinationPath flask_app.zip -Force"
            }
        }

        stage('5. Deploy App') {
            steps {
                echo "Deploying App..."
                bat """
                echo Flask app build complete and ready to run!
                """
            }
        }
    }

    post {
        always {
            echo "Pipeline finished."
        }
    }
}
