pipeline {
    agent any

    // Определение параметров
    parameters {
        string(name: 'BRANCH_NAME', defaultValue: 'default_value', description: 'Название ветки')
        string(name: 'STRING_PARAM', defaultValue: 'default_value', description: 'Пример строкового параметра')
        text(name: 'TEXT_PARAM', defaultValue: 'Это многострочный текст.\nМожно писать несколько строк.', description: 'Пример текстового параметра')
        booleanParam(name: 'BOOLEAN_PARAM', defaultValue: true, description: 'Пример булевого параметра')
        choice(name: 'CHOICE_PARAM', choices: ['Option1', 'Option2', 'Option3'], description: 'Пример параметра-выбора')
        password(name: 'PASSWORD_PARAM', defaultValue: 'secret', description: 'Пример параметра-пароля')
    }

    // Определение переменных окружения
    environment {
        MY_ENV_VAR = 'my_value'
        ANOTHER_ENV_VAR = "${params.STRING_PARAM}" // Использование параметра в переменной окружения
    }

    // Определение опций
    options {
        timeout(time: 1, unit: 'HOURS') // Таймаут для пайплайна
        retry(3) // Количество попыток перезапуска пайплайна в случае ошибки
        disableConcurrentBuilds() // Запрет на параллельное выполнение пайплайна
    }

    stages {
        // Стадия вывода параметров в консоль
        stage('Print Parameters') {
            steps {
                echo "STRING_PARAM: ${params.STRING_PARAM}"
                echo "TEXT_PARAM: ${params.TEXT_PARAM}"
                echo "BOOLEAN_PARAM: ${params.BOOLEAN_PARAM}"
                echo "CHOICE_PARAM: ${params.CHOICE_PARAM}"
                echo "PASSWORD_PARAM: ${params.PASSWORD_PARAM}"
                echo "MY_ENV_VAR: ${env.MY_ENV_VAR}"
                echo "ANOTHER_ENV_VAR: ${env.ANOTHER_ENV_VAR}"
            }
        }

        // Стадия выгрузки кода из репозитория
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Kornyshev/jenkins-demo.git', branch: params.BRANCH_NAME
            }
        }

        // Стадия запуска команды Maven для сборки, тестирования и генерации отчёта
        stage('Build and Test') {
            steps {
                bat 'mvn clean test'
            }
        }

        // Стадия генерации Allure отчёта
        stage('Generate Allure Report') {
            steps {
                allure includeProperties: false, jdk: '', results: [[path: 'target/allure-results']]
            }
        }
    }

    post {
        // Действия после завершения всех стадий
        always {
            echo 'Пайплайн завершён.'
            cleanWs()
        }
        success {
            echo 'Пайплайн выполнен успешно!'
        }
        failure {
            echo 'Пайплайн завершился с ошибкой.'
        }
    }

}