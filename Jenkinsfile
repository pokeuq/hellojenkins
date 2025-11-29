pipeline {
    agent any
    
    parameters {
        string(name: 'STUDENT_NAME', defaultValue: 'hello-bezhan-container', description: 'Имя студента')
        string(name: 'PORT', defaultValue: '8059', description: 'Порт')
    }
    
    stages {
        stage('Удаляем старые контейнеры и образы') {
            steps {
                script {
                    sh "docker ps -a -q --filter name=hello-bezhan-container | xargs -r docker rm -f"
                    sh "docker images -q hello-bezhan-app | xargs -r docker rmi -f"
                }
            }
        }
        stage('Выгружаем код из репозитория') {
            steps {
                git branch: 'main', url: 'https://github.com/pokeuq/hellojenkins.git'
            }
        }
        stage('Собираем docker image') {
            steps {
                script {
                    dockerImage = docker.build("hello-bezhan-app")
                }
            }
        }
        stage('Запускаем тесты в докере') {
            steps {
                script {
                    dockerImage.inside {
                        sh 'python -m unittest test_app.py'
                    }
                }
            }
        }
        stage('Запускаем докер контейнер') {
            steps {
                script {
                    sh "docker run -d --name hello-bezhan-container -p ${params.PORT}:${params.PORT} -e STUDENT_NAME='${params.STUDENT_NAME}' -e PORT=${params.PORT} hello-bezhan-app"
                }
            }
        }
    }
}
