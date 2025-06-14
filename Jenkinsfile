pipeline {
    agent any

    environment {
        PROJECT_DIR = "${env.WORKSPACE}/django-todo"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'develop', url: 'https://github.com/mayuripatkar/django-todo-devops.git'
            }
        }

        stage('Setup Python Env') {
            steps {
                sh '''
                mkdir -p ${PROJECT_DIR}
                cp -r * ${PROJECT_DIR}/
                cd ${PROJECT_DIR}
                python3 -m venv venv
                source venv/bin/activate
                pip install -r requirements.txt || pip install django
                '''
            }
        }

        stage('Migrate DB') {
            steps {
                sh '''
                cd ${PROJECT_DIR}
                source venv/bin/activate
                python manage.py makemigrations
                python manage.py migrate
                '''
            }
        }

        stage('Run Django Server') {
            steps {
                sh '''
                cd ${PROJECT_DIR}
                source venv/bin/activate
                nohup python manage.py runserver 0.0.0.0:8000 &
                '''
            }
        }
    }

    post {
        failure {
            echo 'Something went wrong with the deployment.'
        }
    }
}
