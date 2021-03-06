# JavaScript Style Guide

### 1.Больше не используйте var

Объявляйте все переменные с помощью const или let. По умолчанию используется const, за исключением случаев, кгда нужно переназначить переменную. Ключевое слово var не должно использоваться.

```js
 var example = 42;//плохая практика

 let example = 42;//хорошая практика
```
### 2.Стрелочные функции предпочтительны

Стрелочные функции придают краткость синтаксису и исправляют многие сложности, с ним связанные. Отдавайте предпочтение стрелочным функциям, а не ключевым словам, особенно для вложенных функций.

```js
 [1, 2, 3].map(function (x) {
   const y = x + 1;
   return x * y;
 });//плохой пример

 [1, 2, 3].map((x) => {
   const y = x + 1;
   return x * y;
 });//хороший пример
```
### 3.Объявляйте переменные по одной

Объявление каждой локальной переменной объявляет только одну переменную: такие объявления, как let a = 1, b = 2; не используются.

```js
 let a = 1;
 let b = 2;
 let c = 3;//хороший пример
```
### 4.Используйте строки шаблона вместо конкатенации

Используйте строки шаблона (отделенные с помощью `), а не сложную конкатенацию строк, особенно если задействованы несколько строковых литералов. Строки шаблона могут охватывать несколько строк.

```js
 function sayHi(name) {
   return 'How are you, ' + name + '?';
 };

 function sayHi(name) {
   return ['How are you, ', name, '?'].join();
 };

 function sayHi(name) {
   return `How are you, ${ name }?`;
 };// плохие примеры кода



 function sayHi(name) {
   return `How are you, ${name}?`;
 };//хороший пример
```
### 5. Используйте понятные имена переменных и функций

Код гораздо легче читать, когда при его написании используют понятные, описательные имена функций и переменных. Вот не очень понятный код:

```js
 function avg (a) {
   let s = a.reduce((x, y) => x + y);
   return s / a.length;
 }
```

Его читабельность значительно улучшится, если использовать в нём понятные имена переменных и функций, отражающие их смысл.

```js
 function averageArray (array) {
   let sum = array.reduce((number, currentSum) => number + currentSum);
   return sum / array.length;
 }
```

Используйте полные имена переменных, которые тот, кто будет работать с вашим кодом в будущем, легко сможет понять.

### 6. Пишите короткие функции, решающие одну задачу

Функции легче поддерживать, они становятся гораздо более понятными, читабельными, если они направлены на решение лишь какой-то одной задачи. Если мы сталкиваемся с ошибкой, то, при использовании маленьких функций, найти источник этой ошибки становится гораздо легче. Кроме того, улучшаются возможности по повторному использованию кода. 
Вышеприведенную функцию можно разбить на две функции, тогда роль каждого фрагмента кода станет более понятной.

```js
 function sumArray (array) {
   return array.reduce((number, currentSum) => number + currentSum);
 }

 function averageArray (array) {
   return sumArray(array) / array.length;
 }
```
### 7. Документируйте код
Пишите хорошую документацию к своему коду — тогда тот, кто столкнётся с ним в будущем, поймёт, что и почему в этом коде делается. Вот пример неудачной функции:

```js
 function areaOfCircle (radius) {
   return 3.14 * radius ** 2;
 }
```
Сюда можно добавить комментарии для того, чтобы этот код стал бы более понятным для того, кто не знает формулы для вычисления площади круга.

```js
 const PI = 3.14; // Число Пи, округлённое до двух знаков после запятой

 function areaOfCircle (radius) {
   // Функция реализует математическую формулу вычисления площади круга:
   // Число ПИ умножается на квадрат радиуса круга
   return PI * radius ** 2
 }
```
Комментарии к коду должны отвечать на вопрос «почему».
Обратите внимание на то, что для целей документирования кода имеет смысл воспользоваться специальными инструментами и соответствующими им правилами комментирования кода. В применении к JavaScript это JSDoc.

### 8. Не используйте много уровней вложенности

Уровней вложенности должно быть немного. 
Например, в цикле бывает полезно использовать директиву continue, чтобы избежать лишней вложенности.

Например, вместо добавления вложенного условия if, как здесь:

```js
 for (let i = 0; i < 10; i++) {
   if (cond) {
    ... // <- ещё один уровень вложенности
   }
 }
```
Мы можем написать:

```js
 for (let i = 0; i < 10; i++) {
   if (!cond) continue;
   ...  // <- нет лишнего уровня вложенности
 }
```
### 9. Используйте идеи инкапсуляции и модульности

Группируйте связанные переменные и функции для того, чтобы сделать ваш код понятнее и улучшить его с точки зрения его повторного использования. Вот пример не самого удачно организованного кода, в котором сведения о человеке оформлены в виде отдельных переменных.

```js
 let name = 'Ali';
 let age = 24;
 let job = 'Software Engineer';

 let getBio = (name, age, job) => `${name} is a ${age} year-old ${job}`
```

Если в подобной программе нужно обрабатывать данные многих людей, тогда в ней лучше будет использовать нечто вроде следующей конструкции.

```js
 class Person {
   constructor (name, age, job) {
     this.name = name;
     this.age = age;
     this.job = job;

   getBio () {
     return `${this.name} is a ${this.age} year-old ${this.job}`
   }
 }
```
А если в программе надо работать лишь с данными об одном человеке, то их можно оформить, как объект.

```js
 const ali = {
   name: 'Ali',
   age: 24,
   job: 'Software Engineer',
   getBio: function () {
     return `${this.name} is a ${this.age} year-old ${this.job}`
   }
 }
```
Похожим образом следует подходить к разбиению длинных программ на модули, на отдельные файлы. Это облегчит использование кода, выделенного в отдельные файлы, в разных проектах. В больших файлах с программным кодом часто тяжело ориентироваться, а небольшие понятные модули легко использовать и в том проекте, для которого они созданы, и, при необходимости, в других проектах. Поэтому стремитесь к написанию понятного модульного кода, объединяя логически связанные элементы.

### 10. Подумайте об использований правил Сэнди Метц

Сэнди Метц сформулировала четыре правила написания чистого кода в объектно-ориентированных языках.

1. Классы не должны быть длиннее 100 строк кода.
2. Методы и функции не должны быть длиннее 5 строк кода.
3. Методам следует передавать не более 4 параметров.
4. Контроллеры могут инициализировать лишь один объект.

Данные правила — это лишь рекомендации, но их использование позволит сделать код значительно лучше.



