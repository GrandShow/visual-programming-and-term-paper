# login

### Розробити підсистему генерації JWT токенів доступу на основі form-based механізму аутентифікації.




### Пользовательская история (Use Case) Login
	Страница логина пользователя:
    Как пользователь, я должен иметь возможность залогиниться в системе.
    AC:
        * Когда пользователь нажимает конопку login, он перенаправляется на страницу login которая содержит следующие элементы:
            * текстовое поле - email: required 
            * текстовое поле с маской - password: required 
            * checkbox "запомнить меня" 
            * кнопка "Login" 
            * ссылка "Forgot Password"
            * ссылка  - "Signup"
        * Когда пользователь нажимает кнопку "Login", email и password должны быть провалидированы, email / password / запомнить меня value отправлены на сервер для валидации
            * соответствующее сообщение должно быть отправлдено в случае ошибки 
            * в случае отсутствия ошибок должен быть предоставлен доступ к Dashboard 
                * должен быть сгененирован токен доступа с TTL зависящей от значения флажка "запомни меня" (TTL для обоих опций  долже быть конфигурируем)
                * Если пользователь "деактивирован" предотвратить логин.
                

###План работ
1. OpenAPI spec update PR for api/ims/ims-api.yaml
    login (email and pass)
    refresh
        header "Authorization: Bearer <your_access_token>"
2. имплементация хендлеров

JWT: (таблица users)
    1. email
    2. sub (id)
    3. iat
    4. roles []string{}

1. PR with 
    0. rename signup to ims, 
    rename:
    SignUp_init.down.sql to 000001_create_users_table.down.sql
    SignUp_init.up.sql to 000001_create_users_table.up.sql
    1. model  
    2. SQL script with access_tokens table

2. 
repository:
    GetUserByEmail(email string) (models.User, error)
    GetCredentialByUserID(int userID)(models.Credentials, error)
    InsertAccessToken()

2. Если пользователь прошел проверку успешно, необходимо сгенерировать ему Access и resfresh JWT токен с правильными клеймами.
для генерации токенов используем библиотеку - https://github.com/dgrijalva/jwt-go (https://godoc.org/github.com/dgrijalva/jwt-go#example-NewWithClaims--CustomClaimsType)
3. следующие параметры должны быть конфигурируемы: access token ttl (default 24h), session key salt

4. создать HTTP web хук по получению и заполнению custom claims для JWT токена.(Должно быть конфигурируемо).

5. Реализовать хендлер перевыпуска Access токена с помощью refresh токена 
6. реализовать возможноть выпуска рефреш токена с более длинным временем жизни (remember me)
1. Создать форму где пользователь сможет ввести логин и пароль (галочка, запомнить меня)

deleted = true
soft delete + anonymization
	
	
1. web server на go (GET/POST) ссылка на github repository
    1.а с сохранением в массив (map)
    1.б с сохранением в/ чтением из базы
2. основы sql (select, update, join)
3. Дизайн API
4. Как хешировать пароли

5. создать форму смены пароля где пользователь может если он залогинен, ввести старый пароль и новый пароль который он хочет исполтзовать. 
6. хендлер проверки текущего токена, старого пароля, установки нового пароля и перегенерации токена