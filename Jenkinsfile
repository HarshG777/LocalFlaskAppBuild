pipeline {
    agent { label 'agent1' }
    stages {
        stage('Clone Repository') {
            steps {
                // Clean up previous app instance and clone the repository
                sh 'rm -rf flask-app || true'
                sh 'git clone -b master https://github.com/HarshG777/LocalFlaskAppBuild.git'
                echo "Repository cloned successfully."
            }
        }
        stage('Install Dependencies') {
            steps {
                dir('flask-app') {
                    // Install the dependencies listed in requirements.txt
                    sh 'pip3 install -r requirements.txt'
                    echo "Dependencies installed."
                }
            }
        }
        stage('Stop Existing Flask App') {
            steps {
                dir('flask-app') {
                    // Kill any running Flask app instance
                    sh 'pkill -f "python3 app.py" || true'
                    echo "Stopped existing Flask app (if any)."
                }
            }
        }
        stage('Run Flask App') {
            steps {
                dir('flask-app') {
                    // Start the Flask app in the background
                    sh 'nohup python3 app.py > flask.log 2>&1 &'
                    sh 'tail -f flask.log'  // Tail the log to verify it's running
                    echo "Flask app started successfully."
                }
            }
        }
    }
    triggers {
        // Poll SCM every 5 minutes for changes
        pollSCM('H/5 * * * *')
        
        // Build every hour, regardless of code changes
        cron('H * * * *')
    }
}
