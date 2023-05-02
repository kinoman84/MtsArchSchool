# Компонентная архитектура
<!-- Состав и взаимосвязи компонентов системы между собой и внешними системами с указанием протоколов, ключевые технологии, используемые для реализации компонентов.
Диаграмма контейнеров C4 и текстовое описание. 
Подробнее: https://confluence.mts.ru/pages/viewpage.action?pageId=375783368
-->
## Контейнерная диаграмма
<!-- Value Stream и слои UI/Svc/Data на диаграммах показывать не надо. -->
```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

SHOW_PERSON_OUTLINE()

AddElementTag("microService", $shape=EightSidedShape(), $bgColor="CornflowerBlue", $fontColor="white", $legendText="microservice")
AddElementTag("storage", $shape=RoundedBoxShape(), $bgColor="lightSkyBlue", $fontColor="white")

Person(speaker, "Speaker", "Любой желающий выступить на конференции")
Person(reviewer, "Reviewer", "Эксперт предметной области")
Person(organizer, "Organizer", "Сотрудник комании")
Person(guest, "Guest", "Любой желающий послушать доклады")
System_Ext(notification, "Notification service", "Сервис уведомлений")
System_Ext(streaming, "Streaming service", "сервис для просмотра онлайн трансляций и записанных конференций")

System_Boundary(conf, "Helloconf") {

   System_Boundary(confPlanning, "Conference") {
      Container(confServ, "Conference service", "Java, Spring Boot", "Сервис информации о конференциях")
      Container(scheduleServ, "Schedule service", "Java, Spring Boot", "Сервис конференций")

      Rel(organizer, confServ, "Оганизует конференции", "HTTPS")
      Rel(organizer, scheduleServ, "Управляет расписанием", "HTTPS")
      Rel(scheduleServ, confServ, "Получение данных о конференции", "JSON, HTTPS")
      Rel(guest, confServ, "Регистрируется на конференцию", "HTTPS")

      Rel(guest, confServ, "Регистрируется на конференцию;\nПросматривает конференции", "HTTPS")
   }

   System_Boundary(review, "Report review") {
      Container(reportServ, "Report service", "Java, Spring Boot", "Сервис докладов")
      Container(reviewtServ, "Review service", "Java, Spring Boot", "Сервис докладов")
      
      Rel(reviewtServ, reportServ, "Получает данные о докладе", "JSON, HTTPS")
      Rel(speaker, reportServ, "Подаёт доклад на конференцию", "HTTPS")
      Rel(reviewer, reviewtServ, "Оценивает качество докладос с экспертной точки зрения", "HTTPS")
   }

   System_Boundary(feedback, "Collecting feedback") {
      Container(feedbackServ, "Feedback service", "Java, Spring Boot", "Сервис обратной связи")
      Container(confArhive, "Conference arhive service", "Java, Spring Boot", "Архив конференций")

      Rel(guest, feedbackServ, "Оценивает доклад", "HTTPS")
      Rel(confArhive, feedbackServ, "Получает информацию о отзывах", "HTTPS")
   }
}

Rel(guest, confArhive, "Просматривает прошедшие конференции")

Rel(confServ, reportServ, "Получает данные о докладе", "JSON, HTTPS")
Rel(confServ, confArhive, "Сообщает о завершении конференции", "Kafka")

Rel(notification, guest, "Направялет напоминание о начале конференции")
Rel(confServ, notification, "Уведомление о начале конференции")
Rel(feedbackServ, notification, "Запрос обратной связи")
Rel(reportServ, streaming, "Использует для просмотра проведённых докладов")
Rel(confArhive, streaming, "Использует для трансляций конференции")
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