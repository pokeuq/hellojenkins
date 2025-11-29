~~~{"variant":"standard","title":"Jenkinsfile for khaymuh","id":"81740"}
pipeline {
    agent any
    parameters {
        string(name: 'STUDENT_NAME', defaultValue: 'bezhan daniel', description: 'ФИО студента')
        string(name: 'PORT', defaultValue: '8080', description: 'Порт приложения')
    }
    environment {
        IMAGE_NAME = "student-khaymuh-app"
        CONTAINER_NAME = "hello-khaymuh-container"
    }
    stages {
        stage('Cleanup') {
            steps {
                script {
                    sh "docker ps -a -q --filter name=${env.CONTAINER_NAME} | xargs -r docker rm -f || true"
                    sh "docker images -q ${env.IMAGE_NAME} | xargs -r docker rmi -f || true"
                }
            }
        }
        stage('Checkout') {
            steps {
                git url: 'https://github.com/pokeuq/hellojenkins.git', branch: 'main'
            }
        }
        stage('Build image') {
            steps {
                script {
                    dockerImage = docker.build(env.IMAGE_NAME)
                }
            }
        }
        stage('Run unit tests in image') {
            steps {
                script {
                    dockerImage.inside {
                        sh 'python -m unittest test_app.py'
                    }
                }
            }
        }
        stage('Run container') {
            steps {
                script {
                    sh "docker run -d --name ${env.CONTAINER_NAME} -p ${params.PORT}:${params.PORT} -e STUDENT_NAME='${params.STUDENT_NAME}' -e PORT=${params.PORT} ${env.IMAGE_NAME}"
                }
            }
        }
    }
    post {
        failure {
            script {
                echo "Сборка провалена. Проверьте Console Output."
            }
        }
    }
}
~~~
