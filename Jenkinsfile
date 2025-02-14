pipeline {
    agent any

    environment {
        VENV_DIR = "venv"
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    checkout scm
                }
            }
        }

        stage('Set up Python Environment') {
            steps {
                bat '''
                python -m venv $VENV_DI
                pip install -r requirements.txt
                '''
            }
        }

        stage('Stop Previous Flask Instance') {
            steps {
                script {
                    bat 'pkill -f "python app.py" || true'  // Stops the previous instance if running
                }
            }
        }

        stage('Run Flask App') {
            steps {
                script {
                    bat '''
                    nohup python app.py > flask.log 2>&1 & echo $! > flask.pid
                    '''
                }
            }
        }
    }

    post {
        always {
            echo 'Build Completed!'
        }
        failure {
            echo 'Build Failed!'
        }
    }
}
