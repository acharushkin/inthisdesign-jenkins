## Подготовка репозитария Jenkins-libs

1. Создать методы для реализации функционала непрерывной интеграции

Шаблон

```groovy
def call(Map buildEnv){

    def connectionString = getConnectionString(buildEnv)  
    
    pipeline {
       
        agent {
            label getParameterValue(buildEnv, 'AGENT')
        }

        environment{
            def PATH_TO_TAMPLATE_BASE       = getParameterValue(buildEnv, 'PATH_TO_TAMPLATE_BASE')
            def SOURCE_PATH                 = getParameterValue(buildEnv, 'SOURCE_PATH')
            def V8VERSION                   = getParameterValue(buildEnv, 'V8VERSION')
            def VRUNNER_CONF                = getParameterValue(buildEnv, 'VRUNNER_CONF')
            def _DB_USER_CREDENTIONALS_ID   = getParameterValue(buildEnv, 'DB_USER_CREDENTIONALS_ID')
            def PROCEDURE_SINTAX_CHECK      = getParameterValue(buildEnv, 'PROCEDURE_SINTAX_CHECK')
            def PROCEDURE_TDD_TEST          = getParameterValue(buildEnv, 'PROCEDURE_TDD_TEST')
            def PROCEDURE_BDD_TEST          = getParameterValue(buildEnv, 'PROCEDURE_BDD_TEST')
            def EMAILS_FOR_NOTIFICATION     = getParameterValue(buildEnv, 'EMAILS_FOR_NOTIFICATION')
        }
    
        stages {
            
            stage("Обновление тестового контура") {
                steps {                      
                    timestamps {
                        script{
                            timeout(20) {
                                prepareBase(buildEnv) 
                            }
                        }
                            
                    }
                }
            }
        }
    }
}

def getDBUserCredentialsId(Map buildEnv) {
    try{
        DB_USER_CREDENTIONALS_ID = _DB_USER_CREDENTIONALS_ID
        return "${DB_USER_CREDENTIONALS_ID}"
    } catch (err) {
        return ""
    }
}

```

Метод getParameterValue(Map buildParams, String keyName)

```groovy
String call(Map buildParams, String keyName){
   
    def defaultParams = getDefaultParams()
    if(env."${keyName}" != null ){
        println "ENV: для ключа ${keyName} найдено значение " +  env."${keyName}"
        return env."${keyName}"
    }else{
        if(buildParams."${keyName}" != null ){
            println "buildParams: для ключа ${keyName} найдено значение " +  buildParams."${keyName}"
            return buildParams."${keyName}"
        }
        if(defaultParams."${keyName}" != ''){
            println "defaultParams: для ключа ${keyName} найдено значение " +  defaultParams."${keyName}"
            return defaultParams."${keyName}"
        }
        println "Значение для ключа не найдено ${keyName} возвращаем пустую строку"
        return new String()
    }
}


def getDefaultParams(){
    return [
        // General
        'IS_FILE_CONTUR': 'true', //Выполнение задачи на файловой базе
        'FILE_BASE_PATH': './build/ib', // 'Путь к файловой базе'
        'USING_DOCKER': 'false', // Выполнять задачу в контейнере DOCKER
        'SEND_EMAIL': 'false', // Рассылка оповещения на почту
        'EMAILS_FOR_NOTIFICATION':"", // Почтовый ящик для уведомлений
        'AGENT': 'all',
        'V8VERSION': "8.3",
        'DB_USER_CREDENTIONALS_ID': "", // Идентификатор Аутентификации пользователя информационной базы
        'SERVER_1C': 'localhost',
        'BASE_NAME': 'test',

        // Gitsync
        'PATH_TO_GITSYNC_CONF': './tools/JSON/gitsync_conf.JSON', // Путь к конфигурационному файлу GITSYNC
        
        //Ci
        'PATH_TO_TAMPLATE_BASE' :'./examples/demo.dt',
        'SOURCE_PATH':'./src/cf', // Путь к шаблону базы.
        'UCCODE': 'locked', // Пароль блокировки информационной базы 
        'LOCK_MESSAGE': 'Регламентные работы Контуром CI',
        'VRUNNER_CONF': 'tools/JSON/vRunner.json',
        'PROCEDURE_SINTAX_CHECK': 'true',
        'PROCEDURE_TDD_TEST': 'true',
        'PROCEDURE_BDD_TEST': 'true'

    ]
}
```

Метод cmdRun(String command)

```
def call(String){
    if (isUnix()) {
        sh "${command}"
    } else {
        bat " chcp 65001\n${command}"
    }
}
```

2. Установить vanessa-runner
3. настроить конфигурационный файл ```tools\JSON\vRunner.json```


## Критерий успешности

1. в библиотеке pipeline-libs существуют требуемые методы.
