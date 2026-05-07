pipeline {
    agent any
    triggers {
        pollSCM('* * * * *')
    }
    stages {
        stage('Fetch Data') {
            steps {
                sh 'cd /home/ubuntu/lab-midterm-sp26 && git pull origin main'
            }
        }
        stage('Train Model') {
            steps {
                sh 'cd /home/ubuntu/lab-midterm-sp26 && python3 -m pip install joblib scikit-learn pandas numpy --break-system-packages && python3 train.py --config config.json'
            }
        }
        stage('Rebuild Docker') {
            steps {
                sh 'docker stop ml-pipeline || true'
                sh 'docker rm ml-pipeline || true'
                sh 'cd /home/ubuntu/lab-midterm-sp26 && docker build -t ml-pipeline .'
                sh 'docker run -d --name ml-pipeline -p 8000:8000 ml-pipeline'
            }
        }
    }
}
