# Контекст решения
<!-- Окружение системы (роли, участники, внешние системы) и связи системы с ним. Диаграмма контекста C4 и текстовое описание. 
Подробнее: https://confluence.mts.ru/pages/viewpage.action?pageId=375783261
-->
```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

LAYOUT_WITH_LEGEND()
SHOW_PERSON_OUTLINE()

Person(speaker, "Speaker", "Любой желающий выступить на конференции")
Person(reviewer, "Reviewer", "Эксперт предметной области")
Person(organizer, "Organizer", "Сотрудник комании")
Person(guest, "Guest", "Любой желающий послушать доклады")
System(conference, "Conference", "Организация и проведение конференций")
System(review, "Report review", "Проверка докладов ревьювером")
System(feedback, "Collecting feedback", "Сбор обратной связи")
System_Ext(notification, "Notification service", "Сервис уведомлений")
System_Ext(streaming, "Streaming service", "сервис для просмотра онлайн трансляций и записанных конференций")


Rel(organizer, conference, "")

Rel(speaker, conference, "")
Rel(speaker, review, "")
Rel(reviewer, review, "")

Rel(review, conference, "")

Rel(guest, conference, "")

Rel(conference, feedback, "")

Rel(guest, feedback, "")

Rel(conference, streaming, "")

Rel(conference, notification, "")
Rel(feedback, notification, "")

@enduml
```
