# Компонентная архитектура
<!-- Состав и взаимосвязи компонентов системы между собой и внешними системами с указанием протоколов, ключевые технологии, используемые для реализации компонентов.
Диаграмма контейнеров C4 и текстовое описание. 
Подробнее: https://confluence.mts.ru/pages/viewpage.action?pageId=375783368
-->
## Контейнерная диаграмма

```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

AddElementTag("microService", $shape=EightSidedShape(), $bgColor="CornflowerBlue", $fontColor="white", $legendText="microservice")
AddElementTag("storage", $shape=RoundedBoxShape(), $bgColor="lightSkyBlue", $fontColor="white")

Person(speaker, "Speaker", "Любой желающий выступить на конференции")
Person(reviewer, "Reviewer", "Эксперт предметной области")
Person(organizer, "Organizer", "Сотрудник комании")
Person(guest, "Guest", "Любой желающий послушать доклады")
System_Ext(notification, "Notification service", "Сервис уведомлений")
System_Ext(streaming, "Streaming service", "сервис для просмотра онлайн трансляций и записанных конференций")

System_Boundary(conf, "Helloconf") {
   Container(app, "Клиентское веб-приложение", "html, JavaScript, Angular", "Web портал")
   
   Container(authServ, "Authorization service", "Java, Spring Boot", "Сервис авторизации", $tags = "microService")
   ContainerDb(authDb, "Users", "PostgreSQL", "Хранение информации о пользователях", $tags = "storage")
   
   Container(confServ, "Conference service", "Java, Spring Boot", "Сервис конференций", $tags = "microService")
   ContainerDb(confDb, "Conferences", "PostgreSQL", "Хранение информации о конференциях]", $tags = "storage")

   Container(reportServ, "Report service", "Java, Spring Boot", "Сервис докладов", $tags = "microService")
   ContainerDb(reportDb, "Reports", "PostgreSQL", "Хранение информации о докладах]", $tags = "storage")
}

Rel(app, authServ, "Автоизация, регистрация", "REST, HTTPS")
Rel(authServ, authDb, "Хранение информации о пользователях", "JDBC, SQL")

Rel(app, confServ, "Работа с конференциями", "REST, HTTPS")
Rel(confServ, confDb, "Хранение информации о конференциях", "JDBC, SQL")

Rel(app, reportServ, "Работа с докладами", "REST, HTTPS")
Rel(reportServ, reportDb, "Хранение информации о докладах", "JDBC, SQL")

Rel(confServ, reportServ, "Получает данные о докладе", "REST, HTTPS")
Rel(reportServ, authServ, "Получает данные о докладчике", "REST, HTTPS")
Rel(confServ, authServ, "Получает данные о пользователе", "REST, HTTPS")

Rel(speaker, app, "Подаёт доклад на конференцию", "Browser, HTTPS")
Rel(reviewer, app, "Оценивает качество докладос с экспертной точки зрения", "Browser, HTTPS")
Rel(organizer, app, "Оганизует конференции", "Browser, HTTPS")
Rel(guest, app, "Регистрируется на конференцию;\nПросматривает конференции", "Browser, HTTPS")
Rel(notification, guest, "Направялет напоминание о начале конференции")
Rel(confServ, notification, "Использует для уведомлений")
Rel(reportServ, streaming, "Использует для просмотра проведённых докладов")
Rel(confServ, streaming, "Использует для трансляций конференции")
SHOW_LEGEND()
@enduml
```

## Список компонентов
| Компонент             | Роль/назначение                                                                                                           |
| :-------------------- | :------------------------------------------------------------------------------------------------------------------------ |
| Authorization service | Сервис авторизации. Хранит информацию о пользователях портала и их ролях. Содержит CRUD методы по работе с пользователями |
| Conference service    | Сервис конференций. Содержит CRUD методы по работе с конференциями                                                        |
| Report service        | Сервис докладов. Содержит CRUD методы по работе с докладами                                                               |