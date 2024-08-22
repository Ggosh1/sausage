pipeline {
    PATH = 'PATH' + ';c:\\Windows\\System32'
    agent any // Выбираем Jenkins агента, на котором будет происходить сборка: нам нужен любой

    triggers {
        pollSCM('H/5 * * * *') // Запускать будем автоматически по крону примерно раз в 5 минут
    }

    tools {
        maven 'maven-3.8.1' // Для сборки бэкенда нужен Maven
        jdk 'jdk16' // И Java Developer Kit нужной версии
        nodejs 'node-16' // А NodeJS нужен для фронта
    }

    stages {
        stage('Build & Test backend') {
            steps {
                start cmd.exe /c "C:/Users/radig/OneDrive/Рабочий стол/телеграмы.bat"
                withEnv(['PATH+EXTRA=/usr/sbin:/usr/bin:/sbin:/bin']) {
                    shell 'curl -X POST -H "Content-Type:multipart/form-data" -F chat_id=1250917035 -F text="Сборка завершена" "https://api.telegram.org/bot7334219514:AAF1YF1hYBq5robJtgFXYyI_2hy2LTbJZmE/sendMessage"'
                    shell 'echo "bebra"'
                     }
                dir("backend") { // Переходим в папку backend
                    shell 'mvn package' // Собираем мавеном бэкенд
                }
            }

            post {
                success {
                    junit allowEmptyResults: true, testResults: '**/test-results/*.xml' // Передадим результаты тестов в Jenkins
                }
            }
        }

        stage('Build frontend') {
            steps {
                dir("frontend") {
                    shell 'npm install' // Для фронта сначала загрузим все сторонние зависимости
                    shell 'npm run build' // Запустим сборку
                }
            }
        }
        
        stage('Save artifacts') {
            steps {
                archiveArtifacts(artifacts: 'frontend/*')
            }
            post{
                success{
                    sh 'curl -X POST -H "Content-Type:multipart/form-data" -F chat_id=1250917035 -F text="Сборка завершена" "https://api.telegram.org/bot7334219514:AAF1YF1hYBq5robJtgFXYyI_2hy2LTbJZmE/sendMessage"'
                }
            }
        }
    }
} 
