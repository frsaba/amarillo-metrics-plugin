pipeline {
    agent {
        docker {
            image 'python:3'
            args '-u root'
        }
    }
    environment {
        GITEA_CREDS = credentials('AMARILLO-JENKINS-GITEA-USER')
        TWINE_REPO_URL = "https://git.gerhardt.io/api/packages/amarillo/pypi"
    }
    stages {
        stage('Create virtual environment') {
            steps {
                echo 'Creating virtual environment'
                sh '''python -m venv .venv
                . .venv/bin/activate'''
                
            }
        }
        stage('Installing requirements') {
            steps {
                echo 'Installing packages'
                sh 'python -m pip install -r requirements.txt'
                sh 'python -m pip install --upgrade build'
                sh 'python -m pip install --upgrade twine'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing'
            }
        }
        stage('Build') {
            steps {
                echo 'Building package'
                sh 'python -m build'
            }
        }
        stage('Publish package') {
            steps {
                sh 'python -m twine upload --skip-existing --verbose --repository-url $TWINE_REPO_URL --username $GITEA_CREDS_USR --password $GITEA_CREDS_PSW ./dist/*'              
            }
        }
    }
    post {
        always {
            echo 'This will always run'
        }
        success {
            echo 'This will run only if successful'
        }
        failure {
            echo 'This will run only if failed'
        }
        unstable {
            echo 'This will run only if the run was marked as unstable'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed'
            echo 'For example, if the Pipeline was previously failing but is now successful'
        }
    }
}