pipeline {
    agent any

    environment {
        VENV_DIR = 'venv'
    }

    stages {
        stage('Setup Virtual Environment') {
            steps {
                bat "python -m venv %VENV_DIR%"
            }
        }

        stage('Install Dependencies') {
            steps {
                bat "call %VENV_DIR%\\Scripts\\activate && pip install -r requirements.txt"
            }
        }

        stage('Run Flask App') {
            steps {
                bat "call %VENV_DIR%\\Scripts\\activate && python app.py"
            }
        }
    }
}
