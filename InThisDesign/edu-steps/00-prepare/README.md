## Подготовка окружения

Допускается локальная установка Jenkins!

1. Устанавливаем [Docker for Windows](https://docs.docker.com/docker-for-windows/install/)
2. Устанавливаем [Onescript](http://oscript.io/downloads/latest/OneScript-1.0.21-setup.exe)
3. Устанавливаем [JRE 8](https://www.oracle.com/technetwork/java/javase/downloads/2133155)
4. Запускаем jenkins командой:
    ```docker run -p 8080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home zhdanovr/jenkins_for-one```
5. Зaходим в админку jenkins по адресу http://localhost:8080 (Логин: Administrator; Пароль: secretPassword)
6. Создаем ноду с лейблом ```windows```, запускаем, не забываем указать кодировку UTF-8 (-Dfile.encoding=UTF-8)
7. Создаем 2 проекта на сервере гитлаб: jenkins-libs и My-Project
8. Создаем файловое хранилище для любой тестовой конфигурации


## Критерий успешности:
1. Jenkins c подключенной нодой
2. 2 проекта для выполнения заданий
4. Хранилище конфигрурации

