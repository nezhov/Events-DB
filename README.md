# Events-DB

Данная программа умеет обрабатывать следующий набор команд:

- **Add** ***dateevent*** — добавить в базу данных пару (***date, event***);

- **Print** — вывести всё содержимое базы данных;

- **Find** ***condition*** — вывести все записи, содержащиеся в базе данных, которые удовлетворяют условию ***condition***;

- **Del** ***condition*** — удалить из базы все записи, которые удовлетворяют условию ***condition***;

- **Last** ***date*** — вывести запись с последним событием, случившимся не позже данной даты.

Условия в командах Find и Del накладывают определённые ограничения на даты и события, например:

- **Find date < 2017-11-06** — найти все события, которые случились раньше 6 ноября 2017 года;

- **Del event != "holiday"** — удалить из базы все события, кроме «**holiday**»;

- **Find date >= 2017-01-01 AND date < 2017-07-01 AND event == "sport event"** — найти всё события «**sport event**», 
случившиеся в первой половине 2017 года;

- **Del date < 2017-01-01 AND (event == "holiday" OR event == "sport event")** — удалить из базы все события «**holiday**» и 
«**sport event**», случившиеся до 2017 года.

В командах обоих типов условия могут быть пустыми: под такое условие попадают все события.

### Реализация класса [Database](database.h)

- Метод Add добавляет указанную пару событие-дата в базу
- Метод FindIf принимает в качестве аргумента лямбда функцию, которую он будет использовать для поиска по базе
- Метод RemoveIf делает то же, что и FindIf, только он не находит, а удаляет события

### Вспомогательный класс [Node](node.h) и его наследники
Данный базовый класс был создан для того, чтобы при создании новых классов, унаследованных от Node, можно было вызвать 
Evaluate, который в свою очередь используется для того, чтобы создать лямбда функцию, передаваемую в методы FindIf и 
RemoveIf.

### [Токенизатор](token.cpp) для парсинга логических операций из входной строки
Логика программы такова - она принимает,на вход строчку с командой, парсит её при помощи токенизатора, если это 
необходимо, создаёт лямбда функцию, после чего вызывает соответсвующий метод у класса Database.

#### Пример ввода
```
Add 2017-11-21 Tuesday
Add 2017-11-20 Monday
Add 2017-11-21 Weekly meeting
Print
Find event != "Weekly meeting"
Last 2017-11-30
Del date > 2017-11-20
Last 2017-11-30
Last 2017-11-01
```
#### И соотвествующий этому вводу вывод программы
```
2017-11-20 Monday
2017-11-21 Tuesday
2017-11-21 Weekly meeting
2017-11-20 Monday
2017-11-21 Tuesday
Found 2 entries
2017-11-21 Weekly meeting
Removed 2 entries
2017-11-20 Monday
No entries
```

#### P.S.
При отладке данной программы часть юнит тестов была взята [отсюда](https://gist.github.com/SergeiShumilin/a030350c6226b8091b57ed0c7ccba779)
### Проект был выполнен в рамках курса "Основы разработки на C++: жёлтый пояс"