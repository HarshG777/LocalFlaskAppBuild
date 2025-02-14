pipeline {
    agent any // Adjust the label as per your Jenkins agent

    environment {
        VENV_DIR = 'venv'  // Virtual environment directory
    }

    stages {
        stage('Clone Repository') {
            steps {
                bat 'rmdir /s /q flask-app || echo "No existing directory to remove."' 
                bat 'git clone -b master https://github.com/HarshG777/LocalFlaskAppBuild.git'
                echo "Repository cloned successfully."
            }
        }

        stage('Setup Virtual Environment') {
            steps {
                dir('flask-app') {
                    bat 'python -m venv %VENV_DIR%'
                    bat "%VENV_DIR%\\Scripts\\activate"
                    bat "%VENV_DIR%\\Scripts\\pip install -r requirements.txt"
                }
            }
        }

        stage('Run Flask App') {
            steps {
                dir('flask-app') {
                    bat "start /B %VENV_DIR%\\Scripts\\python3 app.py > flask.log 2>&1"
                    bat "timeout 5" // Wait for 5 seconds before displaying logs
                    bat "type flask.log"
                }
            }
        }
    }
}
