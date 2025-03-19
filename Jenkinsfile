pipeline {
    agent any

    environment {
        EC2_USER = "ubuntu"  // Change for Amazon Linux if needed
        EC2_HOST = "65.0.109.137"  // Your EC2 instance IP
        APP_DIR = "/home/ubuntu/USER-DETAILS/app"
        SSH_KEY = "/var/lib/jenkins/jenkins.pem"
        APP_PORT = "5000"
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Deploy to EC2') {
            steps {
                script {
                    echo "Copying updated application files to EC2..."
                    sh '''
                    scp -o StrictHostKeyChecking=no -i ${SSH_KEY} -r app/main.py app/templates ${EC2_USER}@${EC2_HOST}:${APP_DIR}/
                    '''

                    echo "Restarting application on EC2..."
                    sh '''
                    ssh -o StrictHostKeyChecking=no -i ${SSH_KEY} ${EC2_USER}@${EC2_HOST} <<EOF
                    sudo pkill -f gunicorn || echo "Gunicorn process not found"
                    cd ${APP_DIR}
                    source /home/ubuntu/USER-DETAILS/venv/bin/activate
                    nohup gunicorn -w 4 -b 0.0.0.0:${APP_PORT} main:app > gunicorn.log 2>&1 &
                    exit
EOF
                    '''
                }
            }
        }
    }

    post {
        success {
            echo '✅ Deployment to EC2 successful!'
        }
        failure {
            echo '❌ Deployment failed. Check the logs for details.'
        }
    }
}

