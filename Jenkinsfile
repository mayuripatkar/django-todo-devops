pipeline {
    agent any

    environment {
        APP_DIR = "/opt/django-todo"
        VENV_DIR = "${APP_DIR}/venv"
        PROJECT_DIR = "${env.WORKSPACE}/django-todo"
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/mayuripatkar/django-todo-devops.git', branch: 'develop'
            }
        }

        stage('Setup Python Env') {
            steps {
                sh '''
                    mkdir -p $APP_DIR
                    cp -r * $APP_DIR
                    cd $APP_DIR
                    python3 -m venv venv
                    source venv/bin/activate
                    pip install --upgrade pip
                    pip install -r requirements.txt || pip install django
                '''
            }
        }

        stage('Migrate DB') {
            steps {
                sh '''
                    cd $APP_DIR
                    source venv/bin/activate
                    python manage.py makemigrations
                    python manage.py migrate
                '''
            }
        }

        stage('Run Django Server') {
            steps {
                sh '''
                    cd $APP_DIR
                    source venv/bin/activate
                    nohup python manage.py runserver 0.0.0.0:8000 &
                '''
            }
        }
    }

    post {
        success {
            echo 'Django app deployed successfully!'
        }
        failure {
            echo 'Something went wrong with the deployment.'
        }
    }
}
