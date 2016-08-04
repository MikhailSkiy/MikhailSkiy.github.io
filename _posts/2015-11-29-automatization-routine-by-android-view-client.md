---
layout: post
title: "Автоматизация рутинных задач при Android-разработке или восстание машин на Python"
date: 2015-11-29
---


 При разработке мобильных приложений очень часто приходится повторять одни и теже действия по несколко раз, например при разработке той или иной задачи
 нужно постоянно логиниться, вводить данные тестового аккаунта, пароль, переходить с домашнего экрана на экран со списком элементов и проверить что ваша
 фича работает. Порой это может быть немного утомляюще. В этом посте я расскажу как можно автоматизировать некторые рутинные действия с помощью билиотеки [AndroidViewClient](https://github.com/dtmilano/AndroidViewClient) и скрипта на Python.

 Итак, начнем.

 Совсем недавно у меня появилась задача - написать приложение или скрипт, позволяющий сделать скриншоты всех экранов
Android-приложения. Как известно в AndroidStudio вполне можно сделать скриншот текущего экрана на эмуляторе или подключенном девайсе, но вот всего приложения сразу - врядли. Итак, погуглив некоторое время, я понял, что данную задачу можно решить двумя способами. Первый - использование стандартных утилит, входящих в Android SDK - adb и aapt. Второй - это использование библиотеки  AndroidViewClient. Наверняка есть еще куча разных методов, но я рассмотрю только эти два.
Итак, используя инструменты  adb и aapt можно написать консольное приложение, которое будет работать с командной строкой
и поочередно вызывать необходимую команду. Для получения скриншота текущего экрана приложения алгоритм действий следующий:
    1) Получить список всех packages с apk: adb shell pm list packages -f
    2) Сохранить нужную apk на локальный диск: adb pull[путь apk начинающийся с data  и заканчивающийся name_apk.apk] [путь к локальной директории]. Например adb pull /data/app/com.weezlabs.rx_giphy-1/base.apk c:/
    3) Распарсить манифест приложения, чтобы получить список всех активити: aapt dump xmltree c:/base.apk
    AndroidManifest.xml
    4) Стартовать каждую активити: adb shell am start -n com.weezlabs.rx_giphy/.SearchActivity
    5) Сделать скриншот и запустить следующую

На первый взгляд все кажется просто и логично. Но в реальной жизни все всегда не так как думаешь. Для домашних активити, не принимающих никаких аргументов, такой подход приемлем. Но если актвити должны передаваться какие-то параметры то все становится сложнее. Конечно можно использовать adb:
./adb shell am start -n com.example.admin.moviesapp/.activities.MovieDetailsActivity -e movieId 1234
Но, а если параметры сложнее или для той или иной активити нужен зарегестрированный пользователь и какие-нибудь параметры из класса User? Более того такой подход позволяет запускать активити, а что если нам нужно перемещать между фрагментами в ViewPager или у нас есть активити, содержащая разные фрагменты, которые должны меняться в зависимости от
выбранного элемента в NavigationDrawer? Неразрешимых задач нет, и я уверен что можно что-то придумать. Но не хотелось изобретать как обойти те или иные ограничения. Поэтому я стал активно искать другие способы - и наткнулся на библиотеку AndroidViewClient. Данный инструмент позволяет автоматизировать взаимодействие с  Android-приложением.
Таким образом используя эту библиотеку я написал на Python скрипт, позволяющий запускать Android-приложение, входить в приложение с тестового акаунта, и перемещаться по всем нужным мне экранам и фрагментам, а также делать скриншоты. Давайте рассмотрим как выглядит код и какие основные команды нужны для этого.

Прежде всего необходимо установить Android SDK и библиотеку AndroidViewClient.
Android SDK http://developer.android.com/sdk/index.html
AndroidViewClient https://github.com/dtmilano/AndroidViewClient (necessary to download it as zip)
Installation AndroidViewClient https://github.com/dtmilano/AndroidViewClient/wiki
Install Python Image Library http://stackoverflow.com/a/29438193/1462969
Добавить в переменную окружения пути к Android SDK
http://hathaway.cc/post/69201163472/how-to-edit-your-path-environment-variables-on-mac
/Users/Mikhail/Library/Android/sdk/build-tools/22.0.1
/Users/Mikhail/Library/Android/sdk/tools
/Users/Mikhail/Library/Android/sdk/platform-tools
Если все настроено верно, то нужно подключить девайс, установить Android - приложение и можно запускать скрипт
python /Users/Mikhail/PycharmProjects/androidUITest/mymonkey.py
В методе makeScreen(name) нужно поменять путь для сохранения картинок ('/Users/Mikhail/screens/' + name +'.png')

Для Windows:
1) Установить AndroidSDK (перед этим установить  JAVA SE)
2) Установить python
3) Добавить в  PATH C:\Python27
4) Установить setup-tools (https://pypi.python.org/pypi/setuptools) (https://pypi.python.org/pypi/setuptools#windows-simplified) Сохраняем скрипт в текстовый файл и устанавливаем в cmd: python ex_setup.txt Install
5) Добавляем в PATH C:\Python27\Scripts;
6) Устанавливаем в cmd easy_install --upgrade androidviewclient
7) Ставим PIL http://www.pythonware.com/products/pil/
Чтобы проверить Check PIL version
How can I check PIL (Python Image Library) version?
Run and the Python interpreter and try:
>>> import Image
>>> Image.VERSION
'1.1.7'
