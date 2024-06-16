# GoAppGB3


### Описание
Продолжаем развивать написанное приложения. Теперь нам нужно добавить работу с очередью в нашем приложении. 


### Идея новой версии
Мы хотим быстро создать новый объект ссылки, но в фоне сходить по адресу и распарсить некоторые интересные теги, 
например теги, description, title. Эти новые данные нужно добавить или обновить в базе. Теги нужно добавить к старым,
title заменить если он не пустой. Description пока проигнорировать.

### Что нужно сделать
* Обратите внимание на links-srv там мы в отдельной горутине вызываем e.LinkUpdater.Run(ctx).
* После создания ссылки в internal/link/linkgrpc CreateLink нужно отправить сообщение в очередь с id созданной ссылки.
* В internal/link/stories/link-updater нужно слушать сообщения из очереди с id ссылки. После получения сообщения 
  нужно получить объект ссылки из базы данны. Потом вызывать пакет pkg/scrape и передать туда url ссылки. Пакет 
  попробует распарсить немного данных по ссылке. Если получилось распарсить новые данные, то добавить их в объект 
  ссылки и обновить итоговый объект в базе данных. Приоритет Title отдать тому, что получаем из scrape.