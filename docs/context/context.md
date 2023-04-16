# Контекст решения
<!-- Окружение системы (роли, участники, внешние системы) и связи системы с ним. Диаграмма контекста C4 и текстовое описание. 
Подробнее: https://confluence.mts.ru/pages/viewpage.action?pageId=375783261
-->
```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

LAYOUT_WITH_LEGEND()

Person(speaker, "Speaker", "Любой желающий выступить на конференции")
Person(reviewer, "Reviewer", "Эксперт предметной области")
Person(organizer, "Organizer", "Сотрудник комании")
Person(guest, "Guest", "Любой желающий послушать доклады")
System(conf, "Helloconf", "Портал для организации и проведения научно технических конференций")
System_Ext(notification, "Notification service", "Сервис уведомлений")
System_Ext(streaming, "Streaming service", "сервис для просмотра онлайн трансляций и записанных конференций")

Rel(speaker, conf, "Подаёт доклад на конференцию")
Rel(reviewer, conf, "Оценивает качество докладос с экспертной точки зрения")
Rel(organizer, conf, "Оганизует конференции")
Rel(guest, conf, "Регистрируется на конференцию;\nПросматривает конференции")
Rel(notification, guest, "Направялет напоминание о начале конференции")
Rel(conf, notification, "Использует для уведомлений")
Rel(conf, streaming, "Использует для трансляций")

@enduml
```
