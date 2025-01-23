pipeline {
    agent {
        docker {
            image 'python:3.9-slim'
            args '--user root'
        }
    }
    stages {
        stage('Clone Repository') {
            steps {
                checkout([$class: 'GitSCM',
                          branches: [[name: '*/master']],
                          userRemoteConfigs: [[url: 'https://github.com/bwbayu/simple-python-pyinstaller-app.git']]
                ])
            }
        }
        stage('Build') {
            steps {
                sh '''
                python -m venv venv
                . venv/bin/activate
                pip install --upgrade pip
                pip install pytest
                python -m py_compile sources/add2vals.py sources/calc.py
                '''
            }
        }
        stage('Test') {
            steps {
                sh '''
                . venv/bin/activate
                pytest --verbose sources/test_calc.py
                '''
            }
        }
        stage('Manual Approval'){
            steps{
                input message: 'Lanjutkan ke tahap Deploy?'
            }
        }
        stage('Deploy') {
            steps {
                sh '''
                python sources/add2vals.py 10 20
                sleep 60
                '''
            }
        }
    }
}
