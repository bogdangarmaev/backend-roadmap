## Интеграционные паттерны

Конспект следующей [статьи](https://headspring.com/2018/11/14/integration-patterns-microservices-architecture/), 
о том как интеграционные паттерны могут улучшить вашу микросервисную архитектуру.

### Интеграция через базу данных
Данный паттерн предполагает, что все микросервисы используют единую базу данных. В качестве примера можно 
привести банковское приложение, которое разделено на микросервисы: аутентификация, профиль пользователя, 
транзакции, уведомления и т.д.
![image](https://mlclehz1uim6.i.optimole.com/zGhTKgw.y0By~281e/w:797/h:660/q:90/https://headspring.com/wp-content/uploads/2018/11/Integration-patterns-Database-Integration.png)
Одним из главных преимуществ является то, что данный подход простой. Данный шаблон широко распространен. Однако,
при использовании данного шаблона, необходимо учитывать, что его сложно масштабировать, так как, вся 
производительность будет упираться в базу данных. Даже в случае увеличения мощности сервера базы данных будет
сложно избежать блокирующих операций, а также конфликта на уровне строк.

### Синхронный вызов API
Данный паттерн предполагает наличие у каждого микросервиса свой базы данных. При этом микросервисы общаются между
собой через API, при этом например при вызове микросервиса транзакции, если ему необходимо получение данных о
профиле пользователя, он будет ожидать ответа от другого соотвествующего микросервиса. 
Преимущество данной системы в том, что можно использовать высокий уровень абстракции и разные микросервисы 
могут быть написаны на разных ЯП и использовать разные БД.
![image](https://mlclehz1uim6.i.optimole.com/zGhTKgw.y0By~281e/w:697/h:461/q:90/https://headspring.com/wp-content/uploads/2018/11/Integration-Patterns-API.png)

### ETL - извлечение, преобразование загрузка
Данный паттерн предполагает, что между микросервисами существует промежуточное звено, которое в фоновом режиме по 
расписанию, либо по каким то триггерам обновляет данные между микросервисами. Данный поход позволяет уменьшить связанность между сервисами и увеличить
скорость ответа. ETL походит для служб, которым не важна актуальность данных.
![image](https://mlclehz1uim6.i.optimole.com/zGhTKgw.y0By~281e/w:697/h:461/q:90/https://headspring.com/wp-content/uploads/2018/11/Integration-Patterns-ETL.png)

### Обмен сообщениями
Данный паттерн предполагает наличие промежуточного брокера сообщений между сервисами, например RabbitMQ, MSMQ,
Azure Service Bus. В данном примере, сервис транзакции записывает сообщение об изменении баланса и сервисы которые
подписаны на данное сообщение, могут соответствующим образом на него реагировать. 