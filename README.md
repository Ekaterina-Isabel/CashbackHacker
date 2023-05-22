# Домашнее задание к занятию «1.1. Основы автоматизации»

В качестве результата пришлите ссылку на ваш GitHub-проект.

Все задачи этого занятия нужно делать **в одном репозитории**.

## Задача №1 - CashbackHacker

Вы участвуете в проекте CashbackHacker - небольшой сервис, который сообщает пользователю о том, сколько ему нужно "докупить" в рамках конкретной траты, чтобы получить максимальное количетство кэшбека.

Подробнее: кэшбек начисляется за каждую потраченную полную тысячу рублей, поэтому если вы покупаете что-то на 900 рублей, сервис должен посоветовать вам докупить "ещё чего-нибудь" на 100 рублей.

Код сервиса выглядит следующим образом:

```java
package ru.netology.service;

public class CashbackHackService {
    private final int boundary = 1000;

    public int remain(int amount) {
        return boundary - amount % boundary;
    }
}
```

**Вам нужно:** создать проект на базе Gradle, не добавляя в него внешних зависимостей.

Выложите полученный проект на GitHub. Подключите Github Actions.

**Легенда**

Поскольку вы уже умеете работать с JUnit5 вам поручили провести небольшое исследование, в рамках которого попробовать использовать TestNG и JUnit4.

### Часть 1. TestNG

TestNG сравнительно неплохо [документирован](https://testng.org/doc/documentation-main.html).

На этом уровне, **с точки зрения пользователя**, почти всё*, что поменяется - мы подключим другую библиотеку и будем использовать аннотации из неё и assert'ы.

**Что нужно сделать**

Сделайте ветку testng, в которой:

1\. Добавьте в зависимости TestNG:
```groovy
dependencies {
    testImplementation 'org.testng:testng:7.1.0'

}

test {
    useTestNG()
}
```

2\. Напишите простые автотесты (без параметризации) на основании материала следующего раздела.

**Особенности**

На этом уровне для нас поменяется всего три вещи:
1. Аннотация `@Test` должна имеет Fully Qualified Name `org.testng.annotations.Test`
2. Assert'ы расположены в классе `org.testng.Assert` (**обратите внимание**: в TestNG принято `actual, expected` вместо `expected, actual`)
3. Класс и тестовые методы должны иметь модификатор доступа `public`

### Часть 2. JUnit4

JUnit4 (по сравнению с JUnit5) практически не документирован, поэтому всё, что нам доступно - это [JavaDoc](https://junit.org/junit4/javadoc/latest/index.html)'и и [FAQ](https://junit.org/junit4/faq.html).

На этом уровне, **с точки зрения пользователя**, почти всё*, что поменяется - мы подключим другую библиотеку и будем использовать аннотации из неё и assert'ы.

**Что нужно сделать**

Сделайте ветку junit4, в которой:

1\. Добавьте в зависимости JUnit:
```groovy
dependencies {
    testImplementation 'junit:junit:4.13'
}

test {
    useJUnit()
}
```

2\. Напишите простые автотесты (без параметризации)

**Особенности**

На этом уровне для нас поменяется всего три вещи:
1. Аннотация `@Test` должна имеет Fully Qualified Name `org.junit.Test`
2. Assert'ы расположены в классе `org.junit.Assert`
3. Класс и тестовые методы должны иметь модификатор доступа `public`

**Об ошибках**

В сервисе точно есть ошибка, поэтому один из ваших авто-тестов должен падать.


## Задача №2 - JUnit5 и Legacy (необязательно)

Автотесты - это тоже код и он подвержен тем же проблемам, что и обычный код.

Достаточно часто встречается ситуация, когда в вашем проекте достаточно большое "наследие" (legacy) кода автотестов, например, на JUnit4.

Но новые тесты хочется писать, используя JUnit5. Что же делать в этом случае?

**Описание**

JUnit5 представляет из себя комплексный проект, состоящий из трёх частей: JUnit Platform + JUnit Jupiter + JUnit Vintage

![image](https://user-images.githubusercontent.com/79922872/172035386-7d59031d-2382-49bf-9859-480d136ffe73.png)

До этого мы достаточно вольно использовали названия JUnit5 и JUnit Jupiter (фактически, как синонимы), но начиная с этой ДЗ мы должны их различать:

- JUnit Platform отвечает за запуск тестовых фреймворков на JVM и определяет интерфейс [`TestEngine`](https://junit.org/junit5/docs/current/api/org.junit.platform.engine/org/junit/platform/engine/TestEngine.html). Это интерфейс, определяющий "движок" для поиска и запуска тестов. 
- JUnit Jupiter определяет API и реализацию `TestEngine` для запуска тестов, использующих данное API (junit-jupiter-engine уже идёт готовая реализация `JupiterTestEngine`).
- JUnit Vintage предоставляет `TestEngine` для запуска тестов, использующих JUnit4 и JUnit3.

**Что нужно сделать**

Из ветки junit4 создайте ветку junit4-platform, в которой:

1\. Добавьте в зависимости JUnit Vintage:
```groovy
dependencies {
    testImplementation 'junit:junit:4.13'
    testRuntimeOnly 'org.junit.vintage:junit-vintage-engine:5.6.2'
}

test {
    useJUnitPlatform()
}
```

2\. Удостоверьтесь, что ваши тесты запускаются.

3\. Добавьте в зависимости JUnit Jupiter (по факту - замените код из п.1):
```groovy
dependencies {
    testImplementation 'org.junit.jupiter:junit-jupiter-api:5.6.1'
    testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine:5.6.1'

    testImplementation 'junit:junit:4.13'
    testRuntimeOnly 'org.junit.vintage:junit-vintage-engine:5.6.2'
}

test {
    useJUnitPlatform()
}
```

4\. Напишите те же тесты, но с использованием API JUnit Jupiter

5\. Удостоверьтесь, что запускаются и тесты JUnit4, и тесты JUnit Jupiter

6\. Соберите отчёты и запакуйте их в zip-архив. Напоминаем, они должны выглядеть примерно так:
![image](https://user-images.githubusercontent.com/79922872/172035400-de1fba15-f229-46b7-b78d-29aa23c7232f.png)

7\. Создайте в проекте Issue, к которому приложите отчёты (в личном кабинете вам нужно будет отправить ссылку на то issue, что вы создали - для демонстрационного проекта это: https://github.com/netology-code/aqa-hw-sample/issues/1)
