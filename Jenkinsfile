pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                echo 'Creating virtual environment and installing dependencies...'
                sh '''
                python3 -m venv venv
                . venv/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Running unit tests...'
                sh '''
                . venv/bin/activate
                python -m unittest discover -s .
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                sh '''
                mkdir -p python-app-deploy
                cp app.py python-app-deploy/
                '''
            }
        }

        stage('Run Application') {
            steps {
                echo 'Running Flask application in background...'
                sh '''
                . venv/bin/activate
                nohup python python-app-deploy/app.py > python-app-deploy/app.log 2>&1 &
                echo $! > python-app-deploy/app.pid
                '''
            }
        }

        stage('Test Application') {
            steps {
                echo 'Testing deployed application...'
                sh '''
                . venv/bin/activate
                python test_app.py
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs for details.'
        }
    }
}
