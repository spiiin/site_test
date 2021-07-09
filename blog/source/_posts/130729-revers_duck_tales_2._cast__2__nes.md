---
title: 'Реверс Duck Tales 2. Часть 2 [NES]'
tags:
  - nes
  - hack
abbrlink: 821680239
date: 2013-07-29 01:22:00
---
[Начало](http://spiiin.livejournal.com/66511.html)
Доразобрался с форматом уровней для **Duck Tales 2**.

Цвет кодируется сжато по 4 пары бит в одном байте для 4 тайлов.

Тип тайла не кодируется, а вычисляется по его номеру (первые A0 тайлов считаются проходимыми, остальные - непроходимые).

Видеопамять сжата алгоритмом RLE - первый байт означает символ повтора, дальше любая последовательность из 3х или более повторяющихся байт кодируется тройкой байт - (символ повтора, кол-во повторяющихся байт и сам байт для повтора).

Объекты кодируются последовательными тройками байт: 
```
BANK6:9478 level1_line1:   .BYTE $FF              - маркер конца
BANK6:9479                 .BYTE 0                - общая позиция Y экрана
BANK6:947A                 .BYTE  $20,   0, $A0   - (1. сжатая пара из 3х бит X экрана и 5 бит X объекта, уменьшенное в 8 раз, 2 - Y объекта, 3 - номер объекта)
BANK6:947A                 .BYTE  $20,   0, $9B
BANK6:947A                 .BYTE  $29, $AF, $66
BANK6:947A                 .BYTE  $29, $BF, $67
BANK6:947A                 .BYTE  $37, $BF, $33
BANK6:947A                 .BYTE  $54, $57, $23
BANK6:947A                 .BYTE  $60,   0, $9B
BANK6:947A                 .BYTE  $69, $7F, $67
BANK6:947A                 .BYTE  $6B, $7F, $68
BANK6:947A                 .BYTE  $7B, $47, $33
```

Перед списком объектов хранится список абсолютных указателей на каждую строку экрана каждого уровня (6:9424-9476)

Среди объектов попадаются переключатели банков памяти объектов, с помощью которых можно поставить на любом уровне врагов из любой части игры.

Добавил поддержку Утиных Историй 2 в CadEditor, теперь он поддерживает уже 4 игры:
(Darkwing Duck, Duck Tales 1, Duck Tales 2, Chip & Dale).

![cad_editor_v11_4](http://ic.pics.livejournal.com/spiiin/20318251/27244/27244_600.png "cad_editor_v11_4")
[cad\_editor\_v13.zip](https://github.com/spiiin/CadEditor/blob/master/Release/cad_editor_v13.zip)