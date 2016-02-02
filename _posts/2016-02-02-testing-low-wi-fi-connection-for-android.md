---
layout: post
title: "Имитация плохого интернет-соединения для разработки мобильных приложений на Android"
date: 2016-02-02
---


 При разработке мобильных приложений иногда нужно проверить работу приложения в "диких условиях", а именно как оно себя
 поведет при плохом интернет-соединении. При разработке под iOS есть замечательный инструмент Network Link Conditioner.  Как его настроить написано [тут](https://www.natashatherobot.com/simulate-bad-network-ios-simulator/)
 А вот при разработке под Android, я не нашел настолько удобного инструмента. Поэтому решил написать как решил эту проблему. Итак, мы будем использовать утилиту - [Charles](https://www.charlesproxy.com/). В основном ее пользуются
 для отслеживания сетевого трафика. Однако можно и использовать для имитации плохого соединения. Для начала нужно настроить. Как, написано [тут](https://habrahabr.ru/company/redmadrobot/blog/269109/) см "Инструкция по установке Charles на Mac OS X и подключение телефона." ВАЖНО! Не забудьте проделать дополнительные шаги (5), а именно скачать сертификат www.charlesproxy.com/getssl , который будет использоваться при подключении.
 А теперь для имитации плохого соединения переходим в Charles “Proxy” -> Throttling Settings. Как видно в меню здесь можно настроить все нужные параметры и скорость, от 56 kbps до 3G.

 Ссылки
 [Network Link Conditioner](https://www.natashatherobot.com/simulate-bad-network-ios-simulator/)
 [Charles](https://www.charlesproxy.com/)
 [Настройка Charles](https://habrahabr.ru/company/redmadrobot/blog/269109/)
 [Мануал на английском](http://codewithchris.com/tutorial-using-charles-proxy-with-your-ios-development-and-http-debugging/#SetupProxy)
