## Замыкание

Лучшее, что когда-либо было в ДжаваСкрипте это замыкания. В ДжаваСкрипте функция имеет доступ к любой переменной, определённой во внешней области видимости. Замыкания лучше всего объяснить на примерах:

```ts
function outerFunction(arg) {
    var variableInOuterFunction = arg;

    function bar() {
        console.log(variableInOuterFunction); // Доступ к переменной из внешней области
    }

    // Вызов локальной функции чтобы показать, что она имеет доступ к arg
    bar();
}

outerFunction("hello closure!"); // выведет "hello closure!"
```

Как видите, внутренняя функция имеет доступ к переменной (variableInOuterFunction) из внешней области. Переменные во внешней функции замкнуты (или связаны) внутренней функцией. Отсюда и термин **замыкание**. Сама концепция довольно проста и интуитивно понятна.

Теперь самое крутое: внутренняя функция имеет доступ к переменным из внешней области *даже когда внешняя функция была выполнена*. Всё потому, что переменные еще связаны с внутренней функцией и не зависят от внешней функции. Давайте еще раз посмотрим на пример:

```ts
function outerFunction(arg) {
    var variableInOuterFunction = arg;
    return function() {
        console.log(variableInOuterFunction);
    }
}

var innerFunction = outerFunction("hello closure!");

// Заметьте, что outerFunction была выполнена
innerFunction(); // Выведет "hello closure!"
```

### Почему это круто
Это позволяет вам легко составляеть объекты, например, в паттерне "раскрывающийся модуль":

```ts
function createCounter() {
    let val = 0;
    return {
        increment() { val++ },
        getVal() { return val }
    }
}

let counter = createCounter();
counter.increment();
console.log(counter.getVal()); // 1
counter.increment();
console.log(counter.getVal()); // 2
```

На более высоком уровне это дает возможность существовать штукам вроде Node.js (если в вашем мозгу сейчас ничего не щёлкнуло - не переживайте. Это прийдет со временем 🌹):

```ts
// Псевдокод для пояснения концепции
server.on(function handler(req, res) {
    loadData(req.id).then(function(data) {
        // переменная `res` была замкнута и остается доступна
        res.send(data);
    })
});
```
