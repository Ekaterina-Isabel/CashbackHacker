# Домашнее задание к занятию «1.1. Основы автоматизации»

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

**Вам нужно:** 
1. создать проект на базе Gradle, не добавляя в него внешних зависимостей.
1. Выложите полученный проект на GitHub. Подключите Github Actions.

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

В сервисе точно есть ошибка, поэтому один из ваших авто-тестов должен падать.

**Итого:** отправьте на проверку ссылку на гитхаб-репозиторий с вашим проектом.

**Результат выполнения задания:** [ссылка на текущий репозиторий](https://github.com/Ekaterina-Isabel/CashbackHacker), 
[Issue с описанием ошибки](https://github.com/Ekaterina-Isabel/CashbackHacker/issues/1)


## Задача №2 - JUnit5 и Legacy

Автотесты - это тоже код и он подвержен тем же проблемам, что и обычный код.

Достаточно часто встречается ситуация, когда в вашем проекте достаточно большое "наследие" (legacy) кода автотестов, например, на JUnit4.

Но новые тесты хочется писать, используя JUnit5. Что же делать в этом случае?

JUnit5 представляет из себя комплексный проект, состоящий из трёх частей: JUnit Platform + JUnit Jupiter + JUnit Vintage

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

3\. Добавьте в зависимости JUnit Jupiter:
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

6\. Соберите отчёты и запакуйте их в zip-архив.

7\. Создайте в проекте Issue, к которому приложите отчёты

**Результат выполнения задания:** ссылка на [Issue: отчет по результатам прогона автотестов](https://github.com/Ekaterina-Isabel/CashbackHacker/issues/2)
