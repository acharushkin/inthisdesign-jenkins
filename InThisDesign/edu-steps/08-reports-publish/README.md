## Публикация отчетов

1. Реализовать фунционал публикации отчетов junit и allure

## Критерий успешности
1. Отчеты публикуются вне зависимости от результатов прохождения сборочного цикла


```
   post {  //Выполняется после сборки
        always {
            // всегда
        }
        failure {
            // ошибка
        }
        success {
            // успешно
        } 
    }
```