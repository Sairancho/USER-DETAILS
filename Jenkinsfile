pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Sairancho/USER-DETAILS.git'
            }
        }

        stage('Deploy to Local Directory') {
            steps {
                script {
                    echo 'Copying updated application files locally...'
                    sh 'cp -r app/main.py app/templates /home/ubuntu/USER-DETAILS/app/'
                }
            }
        }
    }
}
