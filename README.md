# Frontend - Wskazówki

## HTML

### Semantyka

HTML5 zapewnia nam wiele elementów semantycznych mających na celu dokładne opisanie treści. Upewnij się, że korzystasz z jego bogatego słownictwa.

```html
<!-- bad -->
<div id=main>
  <div class=article>
    <div class=header>
      <h1>Blog post</h1>
      <p>Published: <span>21st Feb, 2015</span></p>
    </div>
    <p>…</p>
  </div>
</div>

<!-- good -->
<main>
  <article>
    <header>
      <h1>Blog post</h1>
      <p>Published: <time datetime=2015-02-21>21st Feb, 2015</time></p>
    </header>
    <p>…</p>
  </article>
</main>
```

Upewnij się, że rozumiesz semantykę używanych elementów. Gorzej jest używać semantycznego
elementu w niewłaściwy sposób niż pozostawić neutralnym.

```html
<!-- bad -->
<h1>
  <figure>
    <img alt=Company src=logo.png>
  </figure>
</h1>

<!-- good -->
<h1>
  <img alt=Company src=logo.png>
</h1>
```

### Zwięzłość

Zachowaj zwięzły kod. Zapomnij o swoich starych nawykach XHTML.

```html
<!-- bad -->
<!doctype html>
<html lang=en>
  <head>
    <meta http-equiv=Content-Type content="text/html; charset=utf-8" />
    <title>Contact</title>
    <link rel=stylesheet href=style.css type=text/css />
  </head>
  <body>
    <h1>Contact me</h1>
    <label>
      Email address:
      <input type=email placeholder=you@email.com required=required />
    </label>
    <script src=main.js type=text/javascript></script>
  </body>
</html>

<!-- good -->
<!doctype html>
<html lang=en>
  <meta charset=utf-8>
  <title>Contact</title>
  <link rel=stylesheet href=style.css>

  <h1>Contact me</h1>
  <label>
    Email address:
    <input type=email placeholder=you@email.com required>
  </label>
  <script src=main.js></script>
</html>
```

### Dostępność

Dostępność powinna być przemyślana. Nie musisz być ekspertem WCAG, aby poprawić swoją stronę internetową, możesz natychmiast rozpocząć od naprawy drobiazgów, które mają ogromną różnicę, takich jak:

* nauki prawidłowego korzystania z atrybutu `alt`
* upewniając się, że twoje linki i przyciski są oznaczone jako takie (`<div class = button>`)
* nie polegają wyłącznie na kolorach w przekazywaniu informacji
* wyraźnym oznaczaniu formantów formularza

```html
<!-- bad -->
<h1><img alt=Logo src=logo.png></h1>

<!-- good -->
<h1><img alt=Company src=logo.png></h1>
```

### Język i kodowanie znaków

Chociaż definiowanie języka jest opcjonalne, zaleca się, aby zawsze deklarować na elemencie głównym.

Standard HTML wymaga, aby strony korzystały z kodowania znaków UTF-8.
To musi zostać zadeklarowane i chociaż może być zadeklarowane w nagłówku HTTP Content-Type, zaleca się, aby zawsze deklarować to na poziomie dokumentu.

```html
<!-- bad -->
<!doctype html>
<title>Hello, world.</title>

<!-- good -->
<!doctype html>
<html lang=en>
  <meta charset=utf-8>
  <title>Hello, world.</title>
</html>
```

### Wydajność

O ile nie ma ważnego powodu do załadowania skryptów przed treścią, nie blokuj
renderingu swojej strony. Jeśli twój arkusz stylów jest ciężki, izoluj style absolutnie
wymagane początkowo i odłóż ładowanie deklaracji wtórnych do osobnego arkusza stylów.
Dwa żądania HTTP są znacznie wolniejsze niż jedno, ale postrzeganie prędkości jest najbardziej
ważnym czynnikiem.

```html
<!-- bad -->
<!doctype html>
<meta charset=utf-8>
<script src=analytics.js></script>
<title>Hello, world.</title>
<p>...</p>

<!-- good -->
<!doctype html>
<meta charset=utf-8>
<title>Hello, world.</title>
<p>...</p>
<script src=analytics.js></script>
```

## CSS

### Średniki

Podczas gdy średnik jest technicznie separatorem w CSS, zawsze traktuj go jako terminator.

```css
/* bad */
div {
  color: red
}

/* good */
div {
  color: red;
}
```

### Box model

Box model powinien idealnie być taki sam dla całego dokumentu. Globalny
`* { box-sizing: border-box; }` jest w porządku, ale nie zmieniaj domyślnego modelu pudełka
na konkretne elementy, jeśli możesz tego uniknąć.

```css
/* bad */
div {
  width: 100%;
  padding: 10px;
  box-sizing: border-box;
}

/* good */
div {
  padding: 10px;
}
```

### Flow

Nie zmieniaj domyślnego zachowania elementu, jeśli możesz tego uniknąć. Zachowaj elementy w
naturalnym przepływie dokumentów, jak możesz. Na przykład, usunięcie spacji poniżej
obrazu nie powininno powodować zmiany domyślnego wyświetlania:

```css
/* bad */
img {
  display: block;
}

/* good */
img {
  vertical-align: middle;
}
```

Podobnie, nie zdejmuj elementu z przepływu, jeśli możesz tego uniknąć.

```css
/* bad */
div {
  width: 100px;
  position: absolute;
  right: 0;
}

/* good */
div {
  width: 100px;
  margin-left: auto;
}
```

### Pozycjonowanie

Istnieje wiele sposobów pozycjonowania elementów w CSS. Preferuj nowoczesne specyfikacje układu
takich jak Flexbox i Grid, i na przykład unikaj usuwania elementów z normalnego obiegu dokumentów
z `position: absolute`.

### Selektory

Minimalizuj selektory ściśle powiązane z DOM. Rozważ dodanie klasy do elementów, które
chcesz dopasować, gdy twój selektor przekroczy 3 strukturalne pseudoklasy, potomek lub
kombinatory rodzeństwa.

```css
/* bad */
div:first-of-type :last-child > p ~ *

/* good */
div:first-of-type .info
```

Unikaj przeciążania selektorów, gdy nie jest to konieczne.

```css
/* bad */
img[src$=svg], ul > li:first-child {
  opacity: 0;
}

/* good */
[src$=svg], ul > :first-child {
  opacity: 0;
}
```

### Szczegółowość

Nie utrudniaj nadpisywania wartości i selektorów. Zminimalizuj użycie `id`
i unikaj `!important`.

```css
/* bad */
.bar {
  color: green !important;
}
.foo {
  color: red;
}

/* good */
.foo.bar {
  color: green;
}
.foo {
  color: red;
}
```

### Overriding

Nadpisywanie stylów utrudnia korzystanie z selektorów i debugowanie. Unikaj, jeśli to możliwe.

```css
/* bad */
li {
  visibility: hidden;
}
li:first-child {
  visibility: visible;
}

/* good */
li + li {
  visibility: hidden;
}
```

### Dziedziczenie

Nie powielaj deklaracji stylu, które można dziedziczyć.

```css
/* bad */
div h1, div p {
  text-shadow: 0 1px 0 #fff;
}

/* good */
div {
  text-shadow: 0 1px 0 #fff;
}
```

### Zwięzłość

Zachowaj zwięzły kod. Użyj właściwości skróconych i unikaj używania wielu właściwości, kiedy
to nie jest potrzebne.

```css
/* bad */
div {
  transition: all 1s;
  top: 50%;
  margin-top: -10px;
  padding-top: 5px;
  padding-right: 10px;
  padding-bottom: 20px;
  padding-left: 10px;
}

/* good */
div {
  transition: 1s;
  top: calc(50% - 10px);
  padding: 5px 10px 20px;
}
```

### Język

Preferuj angielski ponad matematykę.

```css
/* bad */
:nth-child(2n + 1) {
  transform: rotate(360deg);
}

/* good */
:nth-child(odd) {
  transform: rotate(1turn);
}
```

### Prefiksy dostawcy

Agresywnie zabijaj przestarzałe prefiksy dostawców. Jeśli chcesz ich użyć, włóż je przed
standardowa właściwość.

```css
/* bad */
div {
  transform: scale(2);
  -webkit-transform: scale(2);
  -moz-transform: scale(2);
  -ms-transform: scale(2);
  transition: 1s;
  -webkit-transition: 1s;
  -moz-transition: 1s;
  -ms-transition: 1s;
}

/* good */
div {
  -webkit-transform: scale(2);
  transform: scale(2);
  transition: 1s;
}
```

### Animacje

Preferuj przejścia zamiast animacji. Unikaj animowania innych właściwości niż
`opacity` oraz `transform`.

```css
/* bad */
div:hover {
  animation: move 1s forwards;
}
@keyframes move {
  100% {
    margin-left: 100px;
  }
}

/* good */
div:hover {
  transition: 1s;
  transform: translateX(100px);
}
```

### Jednostki

W miarę możliwości używaj wartości bezjednostkowych. Preferuj `rem`, jeśli używasz jednostek względnych. Preferuj sekundy
niż milisekundy.

```css
/* bad */
div {
  margin: 0px;
  font-size: .9em;
  line-height: 22px;
  transition: 500ms;
}

/* good */
div {
  margin: 0;
  font-size: .9rem;
  line-height: 1.5;
  transition: .5s;
}
```

### Kolory

Jeśli potrzebujesz przejrzystości, użyj `rgba`. W przeciwnym razie zawsze używaj formatu szesnastkowego.

```css
/* bad */
div {
  color: hsl(103, 54%, 43%);
}

/* good */
div {
  color: #5a3;
}
```

### Rysowanie

Unikaj żądań HTTP, gdy zasoby można łatwo replikować za pomocą CSS.

```css
/* bad */
div::before {
  content: url(white-circle.svg);
}

/* good */
div::before {
  content: "";
  display: block;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background: #fff;
}
```

### Hacks

Nie używaj ich.

```css
/* bad */
div {
  // position: relative;
  transform: translateZ(0);
}

/* good */
div {
  /* position: relative; */
  will-change: transform;
}
```

## JavaScript

### Wydajność

Lepsza czytelność, poprawność i ekspresja, niż wydajność. JavaScript zasadniczo nigdy nie będzie
twoim zatorem wydajności. Zoptymalizuj takie rzeczy, jak kompresja obrazu, dostęp do sieci i przepływ DOM.
Jeśli pamiętasz tylko jedną wytyczną z tego dokumentu, wybierz tę.

```javascript
// bad (albeit way faster)
const arr = [1, 2, 3, 4];
const len = arr.length;
var i = -1;
var result = [];
while (++i < len) {
  var n = arr[i];
  if (n % 2 > 0) continue;
  result.push(n * n);
}

// good
const arr = [1, 2, 3, 4];
const isEven = n => n % 2 == 0;
const square = n => n * n;

const result = arr.filter(isEven).map(square);
```

### Bezstanowość

Staraj się utrzymywać czystość swoich funkcji. Wszystkie funkcje powinny idealnie nie wywoływać efektów ubocznych, nie wykorzystywać zewnętrznych danych i zwracać nowe obiekty zamiast mutować istniejące.

```javascript
// bad
const merge = (target, ...sources) => Object.assign(target, ...sources);
merge({ foo: "foo" }, { bar: "bar" }); // => { foo: "foo", bar: "bar" }

// good
const merge = (...sources) => Object.assign({}, ...sources);
merge({ foo: "foo" }, { bar: "bar" }); // => { foo: "foo", bar: "bar" }
```

### Natives

W miarę możliwości polegaj na metodach natywnych.

```javascript
// bad
const toArray = obj => [].slice.call(obj);

// good
const toArray = (() =>
  Array.from ? Array.from : obj => [].slice.call(obj)
)();
```

### Coercion

Embrace implicit coercion when it makes sense. Avoid it otherwise. Don't cargo-cult.

```javascript
// bad
if (x === undefined || x === null) { ... }

// good
if (x == undefined) { ... }
```

### Loops

Nie używaj pętli, ponieważ zmuszają cię do korzystania ze obiektów mutable. Polegaj na metodach `array.prototype`.

```javascript
// bad
const sum = arr => {
  var sum = 0;
  var i = -1;
  for (;arr[++i];) {
    sum += arr[i];
  }
  return sum;
};

sum([1, 2, 3]); // => 6

// good
const sum = arr =>
  arr.reduce((x, y) => x + y);

sum([1, 2, 3]); // => 6
```
Jeśli nie możesz lub jeśli użycie metody `array.prototype` jest prawdopodobnie nieodpowiednie, użyj rekurencji.

```javascript
// bad
const createDivs = howMany => {
  while (howMany--) {
    document.body.insertAdjacentHTML("beforeend", "<div></div>");
  }
};
createDivs(5);

// bad
const createDivs = howMany =>
  [...Array(howMany)].forEach(() =>
    document.body.insertAdjacentHTML("beforeend", "<div></div>")
  );
createDivs(5);

// good
const createDivs = howMany => {
  if (!howMany) return;
  document.body.insertAdjacentHTML("beforeend", "<div></div>");
  return createDivs(howMany - 1);
};
createDivs(5);
```

Oto [ogólna funkcja pętli](https://gist.github.com/bendc/6cb2db4a44ec30208e86) dzięki czemu rekursja jest łatwiejsza w użyciu.

### Argumenty

Zapomnij o obiekcie `arguments`. Reszta parametru jest zawsze lepszą opcją, ponieważ:

1. ma nazwę, dzięki czemu lepiej rozumiesz argumenty, których oczekuje funkcja
2. to prawdziwa tablica, która ułatwia korzystanie z niej.

```javascript
// bad
const sortNumbers = () =>
  Array.prototype.slice.call(arguments).sort();

// good
const sortNumbers = (...numbers) => numbers.sort();
```

### Apply

Zapomnij o `apply()`. Zamiast tego użyj operatora spread.

```javascript
const greet = (first, last) => `Hi ${first} ${last}`;
const person = ["John", "Doe"];

// bad
greet.apply(null, person);

// good
greet(...person);
```

### Bindowanie

Nie używaj `bind()` kiedy jest bardziej idiomatyczne podejście.

```javascript
// bad
["foo", "bar"].forEach(func.bind(this));

// good
["foo", "bar"].forEach(func, this);
```
```javascript
// bad
const person = {
  first: "John",
  last: "Doe",
  greet() {
    const full = function() {
      return `${this.first} ${this.last}`;
    }.bind(this);
    return `Hello ${full()}`;
  }
}

// good
const person = {
  first: "John",
  last: "Doe",
  greet() {
    const full = () => `${this.first} ${this.last}`;
    return `Hello ${full()}`;
  }
}
```

### Funkcje wyższego rzędu

Unikaj funkcji zagnieżdżania, gdy nie musisz.

```javascript
// bad
[1, 2, 3].map(num => String(num));

// good
[1, 2, 3].map(String);
```

### Composition

Unikaj wielu zagnieżdżonych wywołań funkcji. Zamiast tego użyj composition.

```javascript
const plus1 = a => a + 1;
const mult2 = a => a * 2;

// bad
mult2(plus1(5)); // => 12

// good
const pipeline = (...funcs) => val => funcs.reduce((a, b) => b(a), val);
const addThenMult = pipeline(plus1, mult2);
addThenMult(5); // => 12
```

### Caching

Cache dla testów funkcji, dużych struktur danych i wszelkich kosztownych operacji.

```javascript
// bad
const contains = (arr, value) =>
  Array.prototype.includes
    ? arr.includes(value)
    : arr.some(el => el === value);
contains(["foo", "bar"], "baz"); // => false

// good
const contains = (() =>
  Array.prototype.includes
    ? (arr, value) => arr.includes(value)
    : (arr, value) => arr.some(el => el === value)
)();
contains(["foo", "bar"], "baz"); // => false
```

### Zmienne

Sprzyjaj `const` nad `let` oraz `let` nad `var`.

```javascript
// bad
var me = new Map();
me.set("name", "Ben").set("country", "Belgium");

// good
const me = new Map();
me.set("name", "Ben").set("country", "Belgium");
```

### Warunki

Preferuj IIFE i zwracaj instrukcje nad if, else if, else i instrukcjami switch.

```javascript
// bad
var grade;
if (result < 50)
  grade = "bad";
else if (result < 90)
  grade = "good";
else
  grade = "excellent";

// good
const grade = (() => {
  if (result < 50)
    return "bad";
  if (result < 90)
    return "good";
  return "excellent";
})();
```

### Iteracja obiektu

Unikaj `for...in` kiedy tylko możesz.

```javascript
const shared = { foo: "foo" };
const obj = Object.create(shared, {
  bar: {
    value: "bar",
    enumerable: true
  }
});

// bad
for (var prop in obj) {
  if (obj.hasOwnProperty(prop))
    console.log(prop);
}

// good
Object.keys(obj).forEach(prop => console.log(prop));
```

### Obiekty jako Mapy

Podczas gdy obiekty mają uzasadnione przypadki użycia, mapy są zwykle lepszym, bardziej wydajnym wyborem. Kiedy masz
wątpliwość, użyj `Map`.

```javascript
// bad
const me = {
  name: "Ben",
  age: 30
};
var meSize = Object.keys(me).length;
meSize; // => 2
me.country = "Belgium";
meSize++;
meSize; // => 3

// good
const me = new Map();
me.set("name", "Ben");
me.set("age", 30);
me.size; // => 2
me.set("country", "Belgium");
me.size; // => 3
```

### Curry

Currying to potężny, ale obcy paradygmat dla wielu programistów. Nie nadużywaj go, ponieważ odpowiednie przypadki użycia są dość nietypowe.

```javascript
// bad
const sum = a => b => a + b;
sum(5)(3); // => 8

// good
const sum = (a, b) => a + b;
sum(5, 3); // => 8
```

### Czytelność

Nie zaciemniaj intencji swojego kodu za pomocą pozornie inteligentnych sztuczek.

```javascript
// bad
foo || doSomething();

// good
if (!foo) doSomething();
```
```javascript
// bad
void function() { /* IIFE */ }();

// good
(function() { /* IIFE */ }());
```
```javascript
// bad
const n = ~~3.14;

// good
const n = Math.floor(3.14);
```

### Ponowne użycie kodu

Nie bój się tworzyć wielu małych, wysoce kompostowalnych funkcji wielokrotnego użytku.

```javascript
// bad
arr[arr.length - 1];

// good
const first = arr => arr[0];
const last = arr => first(arr.slice(-1));
last(arr);
```
```javascript
// bad
const product = (a, b) => a * b;
const triple = n => n * 3;

// good
const product = (a, b) => a * b;
const triple = product.bind(null, 3);
```

### Zależności

Minimalizuj zależności. Third-party to kod, którego nie znasz. Nie ładuj całej biblioteki tylko dla kilku metod, które można tak replikować:

```javascript
// bad
var _ = require("underscore");
_.compact(["foo", 0]));
_.unique(["foo", "foo"]);
_.union(["foo"], ["bar"], ["foo"]);

// good
const compact = arr => arr.filter(el => el);
const unique = arr => [...new Set(arr)];
const union = (...arr) => unique([].concat(...arr));

compact(["foo", 0]);
unique(["foo", "foo"]);
union(["foo"], ["bar"], ["foo"]);
```

> polska [wersja](https://github.com/mbiesiad/frontend-guidelines/blob/pl-lang/README.md) od: [@mbiesiad](https://github.com/mbiesiad)
