# Route Manager Application
## Лабораторная №5
Реализовать консольное приложение, которое реализует управление коллекцией объектов в интерактивном режиме. В коллекции необходимо хранить объекты класса `Route`, описание которого приведено ниже.
### Разработанная программа должна удовлетворять следующим требованиям:

- Класс, коллекцией экземпляров которого управляет программа, должен реализовывать сортировку по умолчанию.
- Все требования к полям класса (указанные в виде комментариев) должны быть выполнены.
- Для хранения необходимо использовать коллекцию типа `java.util.LinkedHashSet`
- При запуске приложения коллекция должна автоматически заполняться значениями из файла.
- Имя файла должно передаваться программе с помощью: **переменная окружения**.
- Данные должны храниться в файле в формате **csv**
- Чтение данных из файла необходимо реализовать с помощью класса `java.io.BufferedReader`.
- Запись данных в файл необходимо реализовать с помощью класса `java.io.BufferedWriter`.
- Все классы в программе должны быть задокументированы в формате javadoc.
- Программа должна корректно работать с неправильными данными (ошибки пользовательского ввода, отсутсвие прав доступа к файлу и т.п.).

### В интерактивном режиме программа должна поддерживать выполнение следующих команд:
- `help` : вывести справку по доступным командам
- `info` : вывести в стандартный поток вывода информацию о коллекции (тип, дата инициализации, количество элементов и т.д.)
- `show` : вывести в стандартный поток вывода все элементы коллекции в строковом представлении
- `add {element}` : добавить новый элемент в коллекцию
- `update id {element}` : обновить значение элемента коллекции, id которого равен заданному
- `remove_by_id id` : удалить элемент из коллекции по его id
- `clear` : очистить коллекцию
- `save` : сохранить коллекцию в файл
- `execute_script file_name` : считать и исполнить скрипт из указанного файла. В скрипте содержатся команды в таком же виде, в котором их вводит пользователь в интерактивном режиме.
- `exit` : завершить программу (без сохранения в файл)
- `add_if_max {element}` : добавить новый элемент в коллекцию, если его значение превышает значение наибольшего элемента этой коллекции
- `remove_lower {element}` : удалить из коллекции все элементы, меньшие, чем заданный
- `history` : вывести последние 6 команд (без их аргументов)
- `remove_by_id id` : удалить элемент из коллекции по его id
- `min_by_distance` : вывести любой объект из коллекции, значение поля distance которого является минимальным
- `max_by_creation_date` : вывести любой объект из коллекции, значение поля creationDate которого является максимальным
- `remove_all_by_distance distance` : удалить из коллекции все элементы, значение поля distance которого эквивалентно заданному

### Формат ввода команд:
- Все аргументы команды, являющиеся стандартными типами данных (примитивные типы, классы-оболочки, String, классы для хранения дат), должны вводиться в той же строке, что и имя команды.
- Все составные типы данных (объекты классов, хранящиеся в коллекции) должны вводиться по одному полю в строку.
- При вводе составных типов данных пользователю должно показываться приглашение к вводу, содержащее имя поля (например, "Введите дату рождения:")
- Если поле является enum'ом, то вводится имя одной из его констант (при этом список констант должен быть предварительно выведен).
- При некорректном пользовательском вводе (введена строка, не являющаяся именем константы в enum'е; введена строка вместо числа; введённое число не входит в указанные границы и т.п.) должно быть показано сообщение об ошибке и предложено повторить ввод поля.
- Для ввода значений null использовать пустую строку.
- Поля с комментарием "Значение этого поля должно генерироваться автоматически" не должны вводиться пользователем вручную при добавлении.

### Описание хранимых в коллекции классов:

```java
public class Route implements Comparable<Route> {
    private int id; //Значение поля должно быть больше 0, Значение этого поля должно быть уникальным, Значение этого поля должно генерироваться автоматически
    private String name; //Поле не может быть null, Строка не может быть пустой
    private Coordinates coordinates; //Поле не может быть null
    private LocalDate creationDate; //Поле не может быть null, Значение этого поля должно генерироваться автоматически
    private LocationFrom from; //Поле не может быть null
    private LocationTo to; //Поле не может быть null
    private Long distance; //Поле не может быть null, Значение поля должно быть больше 1
    }
```
```java
public class Coordinates {
    private Double x; //Максимальное значение поля: 669, Поле не может быть null
    private double y;
}
```
```java
public class LocationFrom {
    private Integer x; //Поле не может быть null
    private float y;
    private double z;
}
```
```java
public class LocationTo {
    private Float x; //Поле не может быть null
    private Long y; //Поле не может быть null
    private String name; //Поле не может быть null
}
```
## Лабораторная №6
Разделить программу из [лабораторной работы №5](https://github.com/aleksei-edu/Programming-Lab-5) на клиентский и серверный модули. Серверный модуль должен осуществлять выполнение команд по управлению коллекцией. Клиентский модуль должен в интерактивном режиме считывать команды, передавать их для выполнения на сервер и выводить результаты выполнения.
### Необходимо выполнить следующие требования:
- Операции обработки объектов коллекции должны быть реализованы с помощью Stream API с использованием лямбда-выражений.
- Объекты между клиентом и сервером должны передаваться в сериализованном виде.
- Объекты в коллекции, передаваемой клиенту, должны быть отсортированы по местоположению
- Клиент должен корректно обрабатывать временную недоступность сервера.
- Обмен данными между клиентом и сервером должен осуществляться по протоколу TCP
- Для обмена данными на сервере необходимо использовать потоки ввода-вывода
- Для обмена данными на клиенте необходимо использовать сетевой канал
- Сетевые каналы должны использоваться в неблокирующем режиме.
### Обязанности серверного приложения:
- Работа с файлом, хранящим коллекцию.
- Управление коллекцией объектов.
- Назначение автоматически генерируемых полей объектов в коллекции.
- Ожидание подключений и запросов от клиента.
- Обработка полученных запросов (команд).
- Сохранение коллекции в файл при завершении работы приложения.
- Сохранение коллекции в файл при исполнении специальной команды, доступной только серверу (клиент такую команду отправить не может).
### Серверное приложение должно состоять из следующих модулей (реализованных в виде одного или нескольких классов):
- Модуль приёма подключений.
- Модуль чтения запроса.
- Модуль обработки полученных команд.
- Модуль отправки ответов клиенту.

Сервер должен работать в **однопоточном** режиме.
### Обязанности клиентского приложения:
- Чтение команд из консоли.
- Валидация вводимых данных.
- Сериализация введённой команды и её аргументов.
- Отправка полученной команды и её аргументов на сервер.
- Обработка ответа от сервера (вывод результата исполнения команды в консоль).
- Команду `save` из клиентского приложения необходимо убрать.
- Команда `exit` завершает работу клиентского приложения.

**Важно!** Команды и их аргументы должны представлять из себя объекты классов. Недопустим обмен "простыми" строками. Так, для команды add или её аналога необходимо сформировать объект, содержащий тип команды и объект, который должен храниться в вашей коллекции.

### Дополнительное задание:
Реализовать логирование различных этапов работы сервера (начало работы, получение нового подключения, получение нового запроса, отправка ответа и т.п.) с помощью **SLFJ4**.

## Лабораторная №7

**Доработать программу из [лабораторной работы №6](https://github.com/aleksei-edu/Programming-Lab-6) следующим образом:**

1. Организовать хранение коллекции в реляционной СУБД (PostgresQL).
2. Убрать хранение коллекции в файле. 
3. Для генерации поля id использовать средства базы данных (sequence). 
4. Обновлять состояние коллекции в памяти только при успешном добавлении объекта в БД 
5. Все команды получения данных должны работать с коллекцией в памяти, а не в БД 
6. Организовать возможность регистрации и авторизации пользователей. У пользователя есть возможность указать пароль. 
7. Пароли при хранении хэшировать алгоритмом `SHA-256`
8. Запретить выполнение команд не авторизованным пользователям. 
9. При хранении объектов сохранять информацию о пользователе, который создал этот объект. 
10. Пользователи должны иметь возможность просмотра всех объектов коллекции, но модифицировать могут только принадлежащие им. 
11. Для идентификации пользователя отправлять логин и пароль с каждым запросом.

**Необходимо реализовать многопоточную обработку запросов.**
1. Для многопоточного чтения запросов использовать `Fixed thread pool`
2. Для многопоточной обработки полученного запроса использовать `ForkJoinPool` 
3. Для многопоточной отправки ответа использовать `Cached thread pool`
4. Для синхронизации доступа к коллекции использовать потокобезопасные аналоги коллекции из `java.util.concurrent`

### Порядок выполнения работы:
1. В качестве базы данных использовать PostgreSQL.
2. Для подключения к БД на кафедральном сервере использовать хост pg, имя базы данных - studs, имя пользователя/пароль совпадают с таковыми для подключения к серверу.

