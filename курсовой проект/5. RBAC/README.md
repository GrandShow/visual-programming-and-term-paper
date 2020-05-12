# Role Based Access Control

### модуль RBAC





### Пользовательская история (Use Case)
As an admin I should be able:
    * Create new Roles based on Role Template
    * Update Roles
    * Delete Roles
Сущность Role -> имя фичи -> списку эндпоинтов
```golang
type Role struct{
    entries []FeatureEntry    
}

type FeatureEntry struct {
    featureName string
    endpoints []Endpoint
    accessGranted bool
}
```

### Техническая история (Technical Story) RoleTemplate generation

Необходимо в любом момент времени на основе скрипта генерировать Role Template и записывать его в базу.
Для описания этой операции нужно добавить в Makefile таргет (вызывает rbacgen с нужными аргументами)
Написать утилиту rbacgen которая принимает массив путей к open API спекам на базе которых нужно генерировать Role Template

```rbacgen --specs ./api/ims/ims-api.yaml ./api/rbac/rbac-api.yaml ./api/timetabling/timetabling-api.yaml --output ./cmd/rbac/gen-role-template.yaml```

при генерации Role Template есть есть эндпоинт без ролей, то нужно выдавать ошибку.

#####authorization middleware
Необходимо на основании https://www.openpolicyagent.org/
написать middleware для mux роутера, которое будет выполнять authorization
https://play.openpolicyagent.org/p/qUkvgJRpIU

1. создать локальный кэш ролей (вычитать из базы)
2. написать rego script 
3 написать middleware с использованием модуля https://pkg.go.dev/github.com/open-policy-agent/opa/rego?tab=doc








###План работ
Любая функиональность это просто набор HTTP хэндлеров. 
    - Фича редактирование пользователя -> `PUT /users/{id}`
1. собрать список всех фич со всех микросервисов и создать Role Template
2. создать rbac сервис CRUD сервис для сущности Role, RoleTemplate
3. http middleware на golang парсить HTTP запросы и принимать решение об аторизации
4. модуль который при получении адейта ролей будет обновлять внутренний авторизационный кеш 
