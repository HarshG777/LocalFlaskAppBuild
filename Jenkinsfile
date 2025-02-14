pipeline {
    agent { label 'agent1' }
    stages {
        stage('Clean Workspace') {
            steps {
                deleteDir()  // Clean the workspace
                echo "Workspace cleaned."
            }
        }
        stage('Clone Repository') {
            steps {
                // Clone the repository again
                sh 'git clone -b master https://github.com/HarshG777/LocalFlaskAppBuild.git'
                echo "Repository cloned successfully."
            }
        }
        stage('Set Up Virtual Environment') {
            steps {
                dir('LocalFlaskAppBuild') {
                    // Create a virtual environment
                    sh 'python3 -m venv venv'
                    // Activate the virtual environment and install dependencies
                    sh '. venv/bin/activate && pip install -r requirements.txt'
                    echo "Virtual environment setup and dependencies installed."
                }
            }
        }
        stage('Stop Existing Flask App') {
            steps {
                dir('LocalFlaskAppBuild') {
                    // Kill any running Flask app instance
                    sh '''
                    echo "Checking for running Flask processes..."
                    ps aux | grep "python3 app.py"  # List running processes
                    pkill -f "python3 app.py" || true
                    echo "Stopped existing Flask app (if any)."
                    '''
                }
            }
        }
        stage('Run Flask App') {
            steps {
                dir('LocalFlaskAppBuild') {
                    // Activate the virtual environment and start the Flask app
                    sh '. venv/bin/activate && nohup python3 app.py > flask.log 2>&1 &'
                    sh 'tail -f flask.log'  // Tail the log to verify it's running
                    echo "Flask app started successfully."

                    // Wait for 10 seconds and check if it's running
                    sh 'sleep 10'

                    // Check and stop the Flask app after 10 seconds
                    sh '''
                    echo "Stopping Flask app..."
                    ps aux | grep "python3 app.py"  # Check if Flask is still running
                    pkill -f "python3 app.py" || true
                    echo "Flask app stopped successfully."
                    '''
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
