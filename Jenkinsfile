pipeline {
    agent { label 'java_agent' }
    stages {
        stage('Clone Repository') {
            steps {
                sh 'rm -rf flask-app || true'
                sh 'git clone -b main https://github.com/arunpandianj/flask-app.git'
                echo "Repository cloned successfully."
            }
        }
        stage('Run') {
            steps {
                dir('flask-app')
                {
                sh 'python3 app.py > flask.log 2>&1 &'
                sh 'tail -f flask.log'
                }
            }
        }
    }
}
