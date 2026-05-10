# 💾 ZX-Spectrum-plus2A-fdd-interface-light

Hardware interface to connect standard 3.5" floppy disk drives (or Gotek emulators) to the ZX Spectrum +2A/2B using the native +3DOS.  
  
Открытый проект (Open-Source) аппаратного интерфейса дисковода гибких дисков (FDD) для компьютеров **ZX Spectrum +2A и +2B** (модели в черном корпусе).  
  
Проект построен на базе классической микросхемы контроллера **μPD765A** (или Intel 8272A) — точно такого же чипа, который использовался в оригинальном ZX Spectrum +3.  
  
Благодаря полному аппаратному копированию архитектуры +3, данный интерфейс обеспечивает **100% нативную совместимость со встроенной операционной системой +3DOS**.

Вам не понадобятся кастомные прошивки ПЗУ, сложные патчи или программные эмуляторы. Достаточно подключить интерфейс к системной шине (Expansion Bus), подсоединить дисковод и сразу использовать штатные команды +3 BASIC для работы с диском (`LOAD "a:"`, `FORMAT` и т.д.).

### Почему этот проект актуален?
Модели Sinclair ZX Spectrum +2A/+2B поставлялись со встроенным кассетным магнитофоном, но получили обновленную материнскую плату и ПЗУ от «дискового» флагмана +3.  
Этот интерфейс устраняет несправедливость и позволяет легко проапгрейдить ваш +2A, подключив к нему как **реальный механический дисковод 3.5"**,  
так и современный эмулятор **Gotek** с прошивкой FlashFloppy.

## 🎛️ Версии печатных плат (PCB)

Проект разработан в двух вариантах исполнения, что позволяет выбрать наиболее удобный способ сборки:

1. **DIP-версия (Классическая)**  
   * Все вспомогательные микросхемы (логика, буферы) выполнены в выводных корпусах DIP.
   * Идеально подходит для начинающих радиолюбителей и легкой сборки в домашних условиях.
   * Содержит расширенный набор конфигурационных перемычек (джамперов) для гибкой настройки под любые дисководы.
  
[Схема](Export/FDD-3H-DIP.pdf) [Монтаж](Export/FDD-3H-DIP.html) [Gerber](Gerber/FDD-3H-DIP_GERBER.zip)
  
  
![](Foto/FDD-3H-DIP-PCB-1.png)
  
![](Foto/FDD-3H-DIP-PCB-2.png)
  
![](Foto/FDD-3H-DIP-FOTO-1.jpg)
  
![](Foto/FDD-3H-DIP-FOTO-2.jpg)  
  
2. **SOP-версия (Компактная)**  
   * Вспомогательная логика переведена на SMD-компоненты (корпуса SOP/SOIC).
   * Более компактный и современный вид платы, требующий базовых навыков SMD-пайки.
   
[Схема](Export/FDD-3H-SOP.pdf) [Монтаж](Export/FDD-3H-SOP.html) [Gerber](Gerber/FDD-3H-SOP_GERBER.zip)
  
  
![](Foto/FDD-3H-SOP-PCB-1.png)
  
![](Foto/FDD-3H-SOP-PCB-2.png)
  
![](Foto/FDD-3H-SOP-FOTO-1.jpg)
  
![](Foto/FDD-3H-SOP-FOTO-2.jpg)  
  
---
  
## 🔌 Конфигурация перемычек (Джамперов) — только для DIP-версии

На DIP-версии платы предусмотрены аппаратные перемычки, которые позволяют адаптировать интерфейс без физической доработки и порчи корпуса самого дисковода:

* **Выбор сигнала READY**: Позволяет нативно переключать логику работы 34-го контакта шлейфа. Вы можете жестко задать сигнал готовности, если используете стандартный ПК-дисковод без встроенной поддержки Shugart-стандарта. (Актуально и для SOP версии)
* **Перемена мест дисководов (Swap A/B)**: Аппаратная смена очередности приводов. Вы можете назначить любой физический дисковод (или эмулятор Gotek) логическим диском `A:` или `B:` без перепайки резисторов на плате самого дисковода.
* **Блокировка выбора стороны (Side Select Lock)**: Позволяет заблокировать выбор стороны диска. Полезно для специфических тестов, работы со старыми односторонними дискетами или при отладке некоторых образов программ.


\## 🔧 PC 3.5" Floppy Drive Modification (Shugart / READY Mod)

Standard PC 3.5-inch floppy drives (Sony, Samsung, Mitsumi, Panasonic) are hardwired to standard PC specifications.  
To make them work with the μPD765 controller, you must perform two hardware modifications on the drive's PCB:

1\. \*\*Change Drive ID from DS1 to DS0\*\* (Spectrum expects Drive A: to be DS0).

2\. \*\*Route the READY signal to Pin 34\*\* (PC drives put `Disk Change` on Pin 34).

Below are instructions for the most common drive models found today.



\---



\### 📸 Sony MPF920 (Most Common)

1\. \*\*Drive ID (DS0)\*\*: Locate the jumper pads labeled `JC30` / `JC31` (or `DS0` / `DS1`) near the data connector. Remove the zero-ohm resistor (or solder bridge) from `DS1` and solder it across the `DS0` pads.

2\. \*\*READY Signal\*\*: 

&#x20;  \* Locate the trace going to \*\*Pin 34\*\* of the IDC connector. Cut this trace to disconnect the `Disk Change` signal.

&#x20;  \* Locate \*\*Pin 5\*\* of the main drive controller chip (or find a pad labeled `RDY`).

&#x20;  \* Solder a thin kynar wire from the `RDY` pad directly to \*\*Pin 34\*\* of the interface connector.



\---



\### 📸 Samsung SFD-321B

1\. \*\*Drive ID (DS0)\*\*: Locate the solder pads labeled `DC0` / `DC1` (or `S0` / `S1`). Move the SMD resistor/bridge from position `1` to position `0`.

2\. \*\*READY Signal\*\*:

&#x20;  \* Near the 34-pin connector, look for configuration pads labeled `DC` (Disk Change) and `RDY` (Ready).

&#x20;  \* By default, a bridge connects the main line to `DC`. Desolder this bridge and move it to the `RDY` position. (No wire cutting required on most revisions!).



\---



\### ⚙️ Alternative: Gotek Floppy Emulator Configuration

If you are using a \*\*Gotek\*\* drive instead of a real mechanical drive, no soldering is required. Flash the drive with \[FlashFloppy firmware](https://github.com) and update your `FF.CFG` configuration file on the USB drive with the following lines:

```ini

interface = shugart

host = dec-shugart

pin34 = rdy

```

\*\*\*



