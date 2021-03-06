---
title: 'Capcom screen compression algorithm [NES]'
tags:
  - nes
  - hack
abbrlink: 749597909
date: 2014-01-31 19:50:00
---
Разобрал второй способ хранения экранов в NES-ромах от Capcom. 
Им описываются отдельные экраны, создавать для которых наборы блоков и макроблоков расточительно. Этим способом рисуется карта выбора уровней в Darkwing Duck (а также карты и некоторые стартовые/межуровневые/финальные экраны игр Chip & Dale, Duck Tales, Duck Tales 2, Tale Spin, Little Mermaid и Megaman).

Отлавливается алгоритм просто - брейк на запись в область экрана в момент включения карты и трейс чуть вперёд показывает, что в регистр X записывается кол-во байт для чтения. Перед этим в роме указывается адрес памяти, в которую будут занесены данные. После окончания цикла считывания линии байт операция повторяется до тех пор, пока не будет встречен байт остановки 0xFF. Примерно таким способом можно описывать сильно разреженные матрицы с большим количеством нулей. Байты данных кодируют описания экрана на самом низком уровне (<http://dendy.migera.ru/nes/g02.html>) - первые 960 байт описывают номера тайлов, загруженных в видеопамять, а оставшиеся 64 кодируют номера палитры (по 2 бита на блоки из 2x2 тайлов).

Длинные серии нулей в сжатом описании пропускаются, поэтому 1024 бита кодируются в роме 924 байтами. Для изменения и сохранения карты обратно в игру необходимо написать запаковщик. Алгоритм запаковки должен быть следующим - надо разбить данные на блоки, между которыми будут находится группы нулей (кол-во нулей в группе должно быть более 6, потому что на описание двух блоков вокруг нулей уйдёт 6 байт управляющих символов, на каждый по 2 байта адреса и 1 байт на кодирование длины блока). Это удобно закодировать в виде рекурсивного алгоритма - находим группу из 6 или более нулей и делим массив данных на две части, далее к каждой части снова применяем алгоритм. После того, как все нули вычеркнуты - выдаём в выходной буфер адрес начала блока, его длину и сам блок.

Запаковка таким способом пережимает исходный архив от Capcom с 924 байт до 894.
![dwd_map_scheme](http://ic.pics.livejournal.com/spiiin/20318251/35732/35732_original.png "dwd_map_scheme")

Код компрессора-декомпрессора:<https://gist.github.com/spiiin/8738491>

Скрин результирующей карты (из хака [Darkwing Duck in Edoropolis](http://spiiin.livejournal.com/74445.html)):
![dwd_map_screen](http://ic.pics.livejournal.com/spiiin/20318251/35894/35894_original.png "dwd_map_screen")