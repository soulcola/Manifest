node {
    stage('Clone repository') {
        // Клонирование репозитория Manifest. Убедитесь, что remote URL в настройках job задан как:
        // git@github.com:soulcola/Manifest.git
        checkout scm
    }

    stage('Update GIT') {
        script {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                // Активируем SSH credentials для операций с Git
                withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                    // Настройка параметров для коммитов
                    sh "git config user.email '8441404@gmail.com'"
                    sh "git config user.name 'soulcola'"

                    // Выводим содержимое файла для отладки
                    sh "cat deployment.yaml"

                    // Патчим файл deployment.yaml: заменяем тег образа на новый, например, 'raj80dockerid/test:${DOCKERTAG}'
                    sh "sed -i 's+soulcola/voting-app.*+soulcola/voting-app:${DOCKERTAG}+g' deployment.yaml"

                    // Выводим изменённое содержимое файла для проверки
                    sh "cat deployment.yaml"

                    // Добавляем изменения и делаем коммит
                    sh "git add ."
                    sh "git commit -m 'Done by Jenkins Job changemanifest: ${env.BUILD_NUMBER}'"

                    // Пушим изменения через SSH (используем remote 'origin')
                    sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/Manifest.git HEAD:main"
                }
            }
        }
    }
}
