# Модель предметной области
<!-- Логическая модель, содержащая бизнес-сущности предметной области, атрибуты и связи между ними. 
Подробнее: https://confluence.mts.ru/pages/viewpage.action?pageId=375782602

Используется диаграмма классов UML. Документация: https://plantuml.com/class-diagram 
-->

```plantuml
@startuml
' Логическая модель данных в варианте UML Class Diagram (альтернатива ER-диаграмме).
@startuml
class User {
    login : string
    hashPassword: string
    name: string
}

class Speaker {
    reports: Report[]
}

class Reviewer {
}

class Organizer{
    
}

class Guest{
    contactEmail: string
}

class Report {
    speakers : Speaker[]
    reviewer: Reviewer
    status: Status
    comments: string[] 
}

enum Status{
    id : int
    name: sting
}

class Conference {
    organizers: Organizer[]
    timetable: Timeslot[]
    startDate: datetime
    endDate: datetime
}

class Timeslot {
    startDate: datetime
    duration: int
    report: Report
}

User <|-- Speaker
User <|-- Reviewer
User <|-- Organizer
User <|-- Guest

Speaker -- "*" Report
Reviewer -- "*" Report
Conference -- "*" Timeslot
Report -- Timeslot
Organizer -- "*" Conference
Guest -- Conference
Report -- Status

@enduml
```
