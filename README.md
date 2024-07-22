# Разработка Telegram-бота для управления деревом категорий на базе Spring Boot и PostgreSQL

## Цель: Создать Telegram-бота, который позволит пользователям создавать, просматривать и удалять дерево категорий
## Основные требования:
Проект должен быть реализован на Spring Boot.
- Для хранения информации о категориях используется база данных
  PostgreSQL.
- Документирование кода.
- Для работы с Telegram API рекомендуется использовать
  библиотеку TelegramBots.
- Для работы с базой данных можно использовать Spring Data JPA.
- Для обработки команд рекомендуется использовать шаблон
  проектирования Command.
- Загрузка, выгрузка данных из Excel

## Критерии оценки:
- Корректность выполнения команд.
- Структура кода, наличие комментариев и документации.
- Обработка возможных исключений и ошибок ввода пользователя.
- Использование принципов ООП и шаблонов проектирования.

# Основные технологии и фреймворки:
- Spring Boot
- Postgresql
- hypersistence-utils-hibernate-62
- telegrambots-abilities
- org.apache.poi

# Используемые сервисы

## Взаимодействие с telegaramBot (from the controller package)
- TelegramBot 
- ResponseHanlder
- MessageApi

### Структура шаблона стратегии
path: service/commands/ (описание см. javaDoc)
- Help
- ViewTree
- AddElement
- RemoveElement
- Download
- Upload

### Services upload & downlod for excel files
path: com/pandev/service/excelService/ (Назначение классов см. javaDoc) 
- APIGroupsNode
- ExcelService
- ServiceParentNode
- ServiceSubNode

#### Правила загрузки данных из внешнего Excel файла
- если нет корневого узла -> создается
- в БД добавляются только НОВЫЕ ЭЛЕМЕНТЫ
- если выполнить дважды операцию upload с одним и тем файлом = состояние БД не изменится

#### Используемые Шаблоны для Excel:
для загрузки и выгрузки данных из Excel требуются специальные шаблоны:   
any-data/extenal-resource/template.xlsx шаблон для download    
any-data/extenal-resource/test-upload-excel.xlsx образец для upload

### ВСЕ команды ассоциированы с соответствующими константами:
path: com/pandev/utils/Constants
- COMD_REMOVE_ELEMENT -> /removeElement
- COMD_ADD_ELEMENT -> /addElement
- COMD_HELP -> /help
- COMD_VIEW_TREE -> /viewTree
- COMD_START -> /start
- COMD_DOWNLOAD -> /download (Выгрузка данных в Excel)
- COMD_UPLOAD -> /upload (Загрузка элементов в БД из Excel)


## Утилиты (utils/**)
path: com/pandev/utils (назначение классов см. javaDoc)
- FileAPI сервис загрузки данных из файлов
- Constants константы
- InitFormatedTreeService
- InitFormatedTreeString
- ParserMessage

## DTO (DataTransreObject)
path: com/pandev/dto (Описание см. javaDoc)
- DTOgroups
- DTOparser
- DTOresult
- GroupsDetails
- RecordDTOexcel


## Репозиторий (repositories)    
  Для скриптов, которые не поддерживаются Hibernate, использованы опции nativeQuery = true

### Структура бизнес-сущности (Groups)
- rootnode id корневого узла
- parentnode id родительского узла
- ordernum индекс размещения узла в контексте корневого узла
- levelnum уровень вложенности
- txtgroup  строковый идентификатор элемента (уникальные значения)

**Для управления структурой БД использовал liquibase** 

Структура Groups содержит всю информацию по древовидной структуре.        
Добавление узла изменяет ordernum для всех ниже лежащих узлов, входящих в структуру своего корневого узла.       
Удаление узла изменяет значения ordernum, начиная с корневого узла.                    


## Модульное тестирование

**path: com/pandev/commands**
- AddElementTest
- DownloadTest
- HelpTest
- RemoveElementTest
- UploadTest
- ViewTreeTree  
 
**path: com/pandev/service**   
- ExcelSeriveTest
- ServiceParentNodeTest
- ServiceSubNodeTest

**path: com/pandev/utils**   
- FileAPITest
- InitFormatedTreeStringTest****

