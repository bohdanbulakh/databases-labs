# Проєктування бази даних

## Модель бізнес-об'єктів

@startuml

left to right direction

    entity User #ffd24d
    entity User.id #ffe699
    entity User.username #ffe699
    entity User.password #ffe699
    entity User.email #ffe699
    entity User.first_name #ffe699
    entity User.last_name #ffe699
    entity User.avatar #ffe699
    entity User.blocked #ffe699

    User.id -d-* User
    User.username -d-* User
    User.password -d-* User
    User.email -d-* User
    User.first_name -d-* User
    User.last_name -d-* User
    User.avatar --* User
    User.blocked --* User


    entity Grant #a64dff
    entity Grant.id #d9b3ff
    entity Grant.permission #d9b3ff

    Grant.id --* Grant
    Grant.permission --* Grant


    entity Role #4d79ff
    entity Role.id #b3c6ff
    entity Role.name #b3c6ff

    Role.id -d-* Role
    Role.name -d-* Role


    entity ProjectMemberRole #4dffff
    entity ProjectMemberRole.id #b3ffff

    ProjectMemberRole.id --* ProjectMemberRole


    entity ProjectMember #a6ff4d
    entity ProjectMember.id #d9ffb3

    ProjectMember.id --* ProjectMember


    entity Project #4dff79
    entity Project.id #b3ffc6
    entity Project.name #b3ffc6
    entity Project.description #b3ffc6
    entity Project.status #b3ffc6

    Project.id -d-* Project
    Project.name -d-* Project
    Project.description -d-* Project
    Project.status -d-* Project

    entity Task #4dffd2
    entity Task.id #b3ffec
    entity Task.name #b3ffec
    entity Task.description #b3ffec
    entity Task.status #b3ffec
    entity Task.deadline #b3ffec

    Task.id -d-* Task
    Task.name -d-* Task
    Task.description -d-* Task
    Task.status -d-* Task
    Task.deadline -d-* Task


    entity TaskComment #4d4dff
    entity TaskComment.id #b3b3ff
    entity TaskComment.text #b3b3ff

    TaskComment.id --* TaskComment
    TaskComment.text --* TaskComment


    entity Assignment #4dff6b
    entity Assignment.id #b3ffc0

    Assignment.id -l-* Assignment


    entity ConnectToProjectRequest #e1ff4d
    entity ConnectToProjectRequest.id #f2ffb3

    ConnectToProjectRequest.id --* ConnectToProjectRequest


    entity SupportRequest #ff4dd3
    entity SupportRequest.id #ffb3ec
    entity SupportRequest.description #ffb3ec
    entity SupportRequest.topic #ffb3ec

    SupportRequest.id -d-* SupportRequest
    SupportRequest.topic -d-* SupportRequest
    SupportRequest.description -d-* SupportRequest

    entity SupportRequestAnswer #ff4d4d
    entity SupportRequestAnswer.id #ffb3b3
    entity SupportRequestAnswer.feedback #ffb3b3

    SupportRequestAnswer.id -d-* SupportRequestAnswer
    SupportRequestAnswer.feedback -d-* SupportRequestAnswer

    User "1,1" -- "0,*" ProjectMember
    Role "1,1" -l- "0,*" Grant

    ProjectMemberRole "0,*" -u- "1,1" Role
    ProjectMemberRole "0,*" -r- "1,1" ProjectMember
    Role "0,*" -u- "1,1" Project
    ProjectMember "0,*" -u- "1,1" Project
    Project "1,1" -u- "0,*" Task
    TaskComment "0,*" -- "1,1" Task
    Task "1,1" -- "0,*" Assignment
    ProjectMember "1,1" -r- "0,*" Assignment
    ConnectToProjectRequest "0,*" -l- "1,1" User
    ConnectToProjectRequest "0,*" --- "1,1" Project
    TaskComment "0,*" -d- "1,1" ProjectMember

    SupportRequest "0,*" -r- "1,1" User
    SupportRequestAnswer "0,*" -r- "1,1" SupportRequest

    ConnectToProjectRequest -[hidden]- Project
    ConnectToProjectRequest -[hidden]- Project
    ConnectToProjectRequest -[hidden]- Project
    ConnectToProjectRequest -[hidden]- Project
    ConnectToProjectRequest -[hidden]- Task
    User  -r[hidden]-  Project
    Role  -u[hidden]-  Project
    Assignment  -u[hidden]-  TaskComment
    Grant -u[hidden]--- SupportRequestAnswer
    TaskComment -[hidden]- Task
    TaskComment -[hidden]- Task
    TaskComment ---[hidden]- Assignment

@enduml

## ER-модель

@startuml

entity User {
+id: uuid
+username: text
+password: text
+email: text
+first_name: text
+last_name: text
+avatar: text
+blocked: bool
}

entity SupportRequest {
+id: uuid
+user_id: uuid
+topic: text
+description: text
}

entity SupportRequestAnswer {
+id: uuid
+support_request_id: uuid
+feedback: text
}

entity ConnectToProjectRequest {
+id: uuid
+user_id: uuid
+project_id: uuid
}

entity ProjectMember {
+id: uuid
+project_id: uuid
+user_id: uuid
}

entity ProjectMemberRole {
+id: uuid
+project_member_id: uuid
+role_id: uuid
}

entity Role {
+id: uuid
+project_id: uuid
+name: text
}

entity Grant {
+id: uuid
+role_id: uuid
+permission: text
}

entity Project {
+id: uuid
+status: text
+name: text
+description: text
}

entity Task {
+id: uuid
+project_id: uuid
+name: text
+description: text
+status: text
+deadline: date
}

entity TaskComment {
+id: uuid
+task_id: uuid
+project_member_id: uuid
+text: text
}

entity Assignment {
+id: uuid
+task_id: uuid
+project_member_id: uuid
}

User "1,1" --d- "0,*" SupportRequest
User "1,1" --d- "0,*" ConnectToProjectRequest
User "1,1" --d- "0,*" ProjectMember

SupportRequest "1,1" --d- "0,*" SupportRequestAnswer

ProjectMember "1,1" --d- "0,*" ProjectMemberRole
ProjectMember "1,1" --d- "0,*" Assignment

ProjectMemberRole "0,*" --d- "1,1" Role

Role "1,1" --d- "0,*" Grant

Project "1,1"  --d- "0,*" ProjectMember
Project "1,1"  --d- "0,*" Role
Project "1,1"  --d- "0,*" ConnectToProjectRequest
Project "1,1"  --d- "0,*" Task

Task "1,1" --d- "0,*" TaskComment
Task "1,1" --d- "0,*" Assignment
TaskComment "0,*" --u- "1,1" ProjectMember

@enduml

## Реляційна схема

![relational schema.png](..%2F..%2F..%2F..%2Frelational%20schema.png)