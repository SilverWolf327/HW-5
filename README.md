# Домашнее задание к лекции. «5. Классы»

## Задача №1. Печатное издание

Как и на прошлом занятии, мы продолжаем погружаться в тему классов. Наша задача — помочь перевести небольшую районную библиотеку в цифровой формат. Начнём с того, что реализуем несколько классов: классы печатных изданий (книг, журналов) и класс самой библиотеки, чтобы работать с печатными изданиями: хранить их, выдавать читателям, принимать обратно.

### Как выполнить задачу

Все печатные издания имеют название, год издания, состоят из определённого количества страниц, а ещё могут портиться.

1. Создайте базовый класс `PrintEditionItem` со свойствами:

   - `name`,
   - `releaseDate`,
   - `pagesCount`,
   - `state`,
   - `type`.

   Конструктор класса должен принимать название (`name`), дату выпуска (`releaseDate`) и количество страниц (`pagesCount`) в качестве аргумента. Состояние (`state`) по умолчанию установите равным `100`, тип `type` пока должен быть равен `null`.

2. Испорченное издание можно подклеить и этим улучшить его состояние. Создайте метод `fix()`, увеличивающий `state` в полтора раза. Метод не должен принимать аргументов.

3. Нельзя улучшить новое издание и бесполезно подклеивать полностью уничтоженное. Создайте «сеттер» для свойства `state`, принимающий в качестве аргумента новое состояние печатного издания (число).

   Если новое состояние меньше `0`, «сеттер» должен записать в свойство `state` значение `0`. Если новое состояние больше `100`, должно быть записано значение `100`. В остальных случаях в свойство `state` записывается переданное в «сеттер» значение.

4. Создайте «геттер», который читает значение свойства `state`.

_Пример использования_

```js
const sherlock = new PrintEditionItem(
  "Полное собрание повестей и рассказов о Шерлоке Холмсе в одном томе",
  2019,
  1008
);

console.log(sherlock.releaseDate); //2019
console.log(sherlock.state); //100
sherlock.fix();
console.log(sherlock.state); //100
```

5. Создайте класс `Magazine`, который будет наследоваться от класса `PrintEditionItem`. Конструктор класса должен принимать такие же параметры, как и класс-родитель. От базового печатного издания журнал отличается только типом. Значение свойства `type` равно `"magazine"`.

6. Создайте класс `Book`, наследующийся от класса `PrintEditionItem`. Конструктор класса должен принимать такие же параметры, как и класс-родитель, а также имя автора книги `author`. Значение свойства `type` равно `"book"`.

7. Создайте классы `NovelBook` для романов, `FantasticBook` для фантастических произведений и `DetectiveBook` для детективов, наследующиеся от класса `Book`. Значения свойства `type` равны `"novel"`, `"fantastic"` и `"detective"` соответственно.

_Пример использования_

```js
const picknick = new FantasticBook(
  "Аркадий и Борис Стругацкие",
  "Пикник на обочине",
  1972,
  168
);

console.log(picknick.author); //"Аркадий и Борис Стругацкие"
picknick.state = 10;
console.log(picknick.state); //10
picknick.fix();
console.log(picknick.state); //15
```

## Задача №2. Библиотека

Теперь с помощью классов можно описать все печатные издания библиотеки. Следующая задача — подготовить библиотеку к работе в цифровом режиме, то есть реализовать возможности хранить книги, выдавать их читателям и принимать обратно.

### Как выполнить задачу

1. Создайте класс `Library` со свойствами:

   - `name`,
   - `books`.

   Конструктор класса должен принимать название библиотеки `name` (строка). Значением свойства `books` должен быть пустой массив.

2. Реализуйте метод `addBook(book)`, который будет в качестве аргумента принимать объект (книгу или журнал). Метод должен добавлять книгу в хранилище `books`, только если состояние `state` книги больше `30`.

3. Создайте метод `findBookBy(type, value)`, который в качестве аргумента будет принимать ключ, по которому идёт поиск (тип, автор, название, год выпуска и пр.) и искомое значение. Метод должен возвращать книгу в случае успеха и `null`, если книга не найдена.

4. Создайте метод `giveBookByName(bookName)`, который будет в качестве аргумента принимать название книги. Если запрошенная книга найдена, метод должен удалять книгу из хранилища `books` и возвращать её. Если книга не была найдена, метод должен возвращать `null`.

_Пример использования_

```js
const library = new Library("Библиотека имени Ленина");

library.addBook(
  new DetectiveBook(
    "Артур Конан Дойл",
    "Полное собрание повестей и рассказов о Шерлоке Холмсе в одном томе",
    2019,
    1008
  )
);
library.addBook(
  new FantasticBook(
    "Аркадий и Борис Стругацкие",
    "Пикник на обочине",
    1972,
    168
  )
);
library.addBook(new NovelBook("Герберт Уэллс", "Машина времени", 1895, 138));
library.addBook(new Magazine("Мурзилка", 1924, 60));

console.log(library.findBookBy("name", "Властелин колец")); //null
console.log(library.findBookBy("releaseDate", 1924).name); //"Мурзилка"

console.log("Количество книг до выдачи: " + library.books.length); //Количество книг до выдачи: 4
library.giveBookByName("Машина времени");
console.log("Количество книг после выдачи: " + library.books.length); //Количество книг после выдачи: 3
```

5. Протестируйте, как работают классы и методы:
   - создайте библиотеку;
   - добавьте в библиотеку несколько печатных изданий различных типов;
   - найдите книгу, изданную в 1919 году. При необходимости создайте такую книгу;
   - выдайте любую книгу;
   - испортите выданную книгу;
   - почините выданную книгу;
   - добавьте починенную книгу обратно в библиотеку.

## Задача №3. Журнал успеваемости*

> Это необязательная задача. Её можно не выполнять. На зачёте это не скажется.

Мы продолжаем работать с журналом успеваемости студентов. Нам нужно **преобразовать код прошлого ДЗ к классам** и добавить возможности:

- внести оценки по названию предмета,
- получить среднюю оценку по предмету,
- получить среднюю оценку по всем предметам,
- вывести сообщение об ошибке при оценке не в промежутке 1-5. Оценка в этом случае не заносится в журнал.

Вам предстоит придумать структуру данных для хранения оценок по предметам. Работать это должно так:

### Пример

```js
class Student {
  // ваш код
}

const student = new Student("Олег Никифоров");
student.addMark(5, "algebra");
student.addMark(5, "algebra");
student.addMark(5, "geometry");
student.addMark(4, "geometry");
student.addMark(6, "geometry"); // "Ошибка, оценка должна быть числом от 1 до 5"
student.getAverageBySubject("geometry"); // Средний балл по предмету geometry 4.5
student.getAverageBySubject("biology"); // Несуществующий предмет
student.getAverage(); // Средний балл по всем предметам 4.75
student.exclude("Исключен за попытку подделать оценки");
```

### Критерии выполнения

- Реализованы все методы, в каждом из которых вы привели примеры работы исключительных случаев: неверный формат оценки, несуществующий предмет.
- Можно добавить оценку по любому предмету.

## Требования к домашней работе:

- браузер;
- редактор кода, например [Sublime][1] или [Visual Studio Code][2];
- аккаунт на [GitHub.][0] [Инструкция по регистрации на GitHub][3];
- система контроля версий [Git][4], установленная локально. [Инструкция по установке Git][5];
- запуск всех тестов должен успешно выполнять все тесты:

![графическое представление](../Jasmine/results/sucessed_tasks5.png)

## Решение задач

1. Произведите [Fork](https://ru.wikipedia.org/wiki/Форк) репозитория с задачами. Fork нужно делать перед выполнением каждой домашней работы.
2. Перейдите в папку задания `cd ./5.classes`.
3. Откройте файл `task.js` в редакторе кода и выполните задание.
4. Самостоятельно вызывать функции не нужно, если об этом не просят в задании.
5. Откройте файл `index.html` в браузере и с помощью консоли DevTools и убедитесь, что результаты выводятся правильно.
6. Откройте файл `test-runer.html` в браузере и убедитесь, что все тесты выполняются. На вкладке Spec List можно видеть, какие тесты выполнились, а какие нет.
7. Добавьте файл `task.js` в индекс git с помощью команды `git add %file-path%`, где %file-path% — путь до целевого файла. `git add task.js`.
8. Сделайте коммит, используя команду `git commit -m '%comment%'`, где `%comment%` — это произвольный комментарий к вашему коммиту. `git commit -m 'first commit variables'`.
9. Опубликуйте код в репозиторий `homeworks` с помощью команды `git push -u origin main`.
10. Прикрепите ссылку на репозиторий в личном кабинете на сайте [Нетологии][6].

[0]: https://github.com/
[1]: https://www.sublimetext.com/
[2]: https://code.visualstudio.com/
[3]: https://github.com/netology-code/guides/blob/master/git/github.md
[4]: https://git-scm.com/
[5]: https://github.com/netology-code/guides/blob/master/git/README.md
[6]: https://netology.ru/

_Никаких файлов прикреплять не нужно._

Присылать на проверку можно каждую задачу по отдельности или все задачи вместе. Во время проверки по частям ваша домашняя работа будет со статусом «На доработке».

Любые вопросы по задачам задавайте в чате учебной группы.