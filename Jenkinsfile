pipeline {
    agent any

    environment {
        VENV_DIR = 'venv'
    }

    stages {
        stage('Setup Virtual Environment') {
            steps {
                script {
                    bat "python -m venv %VENV_DIR%"
                    bat "%VENV_DIR%\\Scripts\\activate"
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                bat "%VENV_DIR%\\Scripts\\pip install -r requirements.txt"
            }
        }

        stage('Run Flask App') {
            steps {
                bat "python app.py"
            }
        }
    }
}
