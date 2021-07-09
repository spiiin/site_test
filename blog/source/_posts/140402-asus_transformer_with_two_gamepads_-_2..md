---
title: Asus Transformer with Two Gamepads - 2
tags:
  - nes
  - soft
  - hack
  - android
abbrlink: 3734994597
date: 2014-04-02 19:44:00
---
Если подключить два проводных геймпада как на видео из [поста](http://spiiin.livejournal.com/75943.html), то появляется проблема с тем, что операционная система планшета выдаёт одинаковые коды нажатий на кнопки крестовины геймпада и их невозможно отличить друг от друга в играх. Проблема упоминается на разных форумах и конкретно в комментариях к видео на ютубе. После нескольких дней ковыряний удалось разобраться с ней. - Скачать/купить приложение [USB/BT Joystick Center GOLD](https://play.google.com/store/apps/details?id=com.poke64738.usbjoygold&hl=ru).

Лично у меня купленная последняя версия работать нормально отказалась и регулярно падала в процессе настройки, пришлось качать с 4pda взломанную 0.968. 

Настройка приложения. Мануал от разработчиков:
Нажать кнопку "Создать драйвер" для каждого из джойстиков.
В настройках драйвера создать кнопки, стики и слайдеры по количеству таковых на джойстике.
Увеличивать ползунок количества байт управления до тех пор, пока при нажатии на любую из кнопок джойстика какие-либо из бит-лампочек будут загораться (у меня получилось 8 и 32).
Для каждой из кнопок джойстика проделать следующее - зажать кнопку на джойстике, и, не отпуская её, выделить на экране все загоревшиеся биты-лампочки. После отпускания нажать ту же кнопку еще раз и убедиться, что все загорающиеся лампочки выделены точками.
После настройки всех кнопок всех джойстиков, на экране добавления джойстиков надо привязать каждую из кнопок джойстиков на какую-либо из клавиш клавиатуры планшета. Делается это также - зажимается кнопка и, не отпуская её, нажимается кнопка на клавиатуре.
После всех настроек очень рекомендуется сохранить конфигурацию драйвера в файл (кнопка Save).
Во время установки программа добавит новое устройство ввода USBJoyIME, чтобы нажатия на джойстик считывались ОС, надо переключиться на него. Напрямую сделать это на Asus Transformer нельзя (при подключенной док-станции, она всегда активируется как единственный доступный метод ввода), но можно включить его кнопкой "IME" из самой программы. Подключенная клавиатура при этом работать перестаёт, зато начинают джойстики.
Дополнительно в эмуляторах Nesoid, Gensoid и Snesoid (у них одинаковое окно настроек), надо зайти в окно Other Settings и включить галочку Use Additional input method.
(Этот вопрос тоже несколько раз проскакивал на форумах).
Теперь в самом эмуляторе или игре остаётся просто зайти в настройки управления и назначить кнопки джойстиков в качестве управляющих.