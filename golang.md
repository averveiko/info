Чтение stdin и вывод
```go
var s string
fmt.Scan(&s)
d, _ := time.ParseDuration(s) // Парсинг строки в формате 2h30m20s
fmt.Printf("%v = %v min", s, d.Minutes())
```

Переменные
```go
var a = "initial"
var b, c int = 1, 2
var d = true

//Оператор := объявляет и инициализирует переменную. var и тип можно не указывать:
f := "apple"  // var f string = "apple"
```

Константы
```go
const s string = "constant"
```

Циклы
```go
// Аналог while
i := 1
for i <= 3 {
    fmt.Println(i)
    i = i + 1
}
// Классический for
for j := 7; j <= 9; j++ {
    fmt.Println(j)
}
// Вечный (до break или return)
for {
    fmt.Println("loop")
    break
}
//continue переходит к следующей итерации цикла
```

If/else
```go
if 7%2 == 0 {
    fmt.Println("7 is even")
} else {
    fmt.Println("7 is odd")
}

// Единственный нюанс: перед условием может идти выражение. Объявленные в нем переменные доступны во всех ветках:
if num := 9; num < 0 {
    fmt.Println(num, "is negative")
} else if num < 10 {
    fmt.Println(num, "has 1 digit")
} else {
    fmt.Println(num, "has multiple digits")
}
```

Switch
```go
// В одной ветке можно указать несколько выражений. Ветка default сработает, если остальные не подошли
switch time.Now().Weekday() {
case time.Saturday, time.Sunday:
    fmt.Println("It's the weekend")
default:
    fmt.Println("It's a weekday")
}
// It's the weekend

// Выражения в ветках не обязательно должны быть константами. switch может работать как if:
t := time.Now()
switch {
case t.Hour() < 12:
    fmt.Println("It's before noon")
default:
    fmt.Println("It's after noon")
}
// It's before noon
```

Массивы
```go
var a [5]int
fmt.Println("empty:", a)
// empty: [0 0 0 0 0]

fmt.Println("len:", len(a))
// len: 5

b := [5]int{1, 2, 3, 4, 5}

var twoD [2][3]int
for i := 0; i < 2; i++ {
    for j := 0; j < 3; j++ {
        twoD[i][j] = i + j
    }
}
fmt.Println("2d:", twoD)
// 2d: [[0 1 2] [1 2 3]]
```

// Срезы
```go
s := make([]string, 3)
fmt.Printf("empty: %#v\n", s)
// empty: []string{"", "", ""}
//Шаблон %#v возвращает «внутреннее представление» значения, примерно как repr() в питоне.

fmt.Println("len:", len(s))
// len: 3

// В отличие от массива, в срез можно добавлять новые элементы через встроенную функцию append(). Функция возвращает новый срез:
fmt.Println("src:", s)
// src: [a b c]

s = append(s, "d")
s = append(s, "e", "f")
fmt.Println("upd:", s)
// upd: [a b c d e f]

dst := make([]string, len(s))
copy(dst, s) // Копирование среза
fmt.Println("copy:", dst)
// copy: [a b c d e f]

// Выражение slice[from:to] вернет срез от элемента с индексом from включительно до элемента с индексом to не включительно:
sl1 := s[2:5]
fmt.Println("sl1:", sl1)
// sl1: [c d e]

// Срез можно инициализировать при объявлении:
t := []string{"g", "h", "i"}
```

Срезы и строки
```go
// Строку можно преобразовать в срез байт и обратно:
str := "го!"
bytes := []byte(str)
fmt.Println(bytes)
// [208 179 208 190 33]
fmt.Println(str == string(bytes))
// true

// Строку можно преобразовать в срез unicode-символов (Go называет их рунами). Одна руна может занимать несколько байт (что и произошло с рунами г и о):
runes := []rune(str)
fmt.Println(runes)
// [1075 1086 33]
fmt.Println(str == string(runes))
// true
```

Карты
Карта (map), так же известная как словарь (dict), хеш-таблица (hash table) или ассоциативный массив (associative array) — это неупорядоченный набор пар «ключ-значение».
```go
m := make(map[string]int)
m["key"] = 7
m["other"] = 13
fmt.Println("map:", m)
// map: map[key:7 other:13]
val := m["key"]

fmt.Println("len:", len(m))

delete(m, "other")

// Обращение к записи по ключу возвращает необязательное второе значение: признак, есть такой ключ в карте или нет. Обращение по несуществующему ключу не приведет к ошибке, но вернет этот признак со значением false:
_, ok := m["other"]
fmt.Println("has other:", ok)
// has other: false

n := map[string]int{"foo": 1, "bar": 2}
```

Обход коллекции (range обходит элементы коллекций)
```go
// Просуммировать элементы среза (или массива):
nums := []int{2, 3, 4}
sum := 0
for _, num := range nums {
    sum += num
}
fmt.Println("sum:", sum)
// sum: 9

// range на карте итерирует по записям:
m := map[string]string{"a": "apple", "b": "banana"}
for key, val := range m {
    fmt.Printf("%s -> %s\n", key, val)
}
// a -> apple
// b -> banana

// Или только по ключам:
for key := range m {
    fmt.Println("key:", key)
}
// key: a
// key: b

// range на строках итерирует по unicode-символам (рунам). Первое значение — порядковый номер байта, с которого начинается руна (руна может занимать несколько байт). Второе значение — числовой код самой руны:
for idx, char := range "ого" {
    fmt.Println(idx, char, string(char))
}
// 0 1086 о
// 2 1075 г
// 4 1086 о
```

Функции
```go
func divide(divisible, divisor int) (int, int) {
    quotient := divisible / divisor
    remainder := divisible % divisor
    return quotient, remainder
}

// Переменное количество аргументов
func sum(nums ...int) {
    fmt.Print(nums, " -> ")
    total := 0
    for _, num := range nums {
        total += num
    }
    fmt.Println(total)
}
// обычный вызов
sum(1, 2, 3)
// а можно раскрыть срез
nums := []int{1, 2, 3, 4}
sum(nums...)
```

Анонимные функции
```go
// Чаще всего анонимные функции используют, чтобы вернуть из функции другую функцию
// intSeq() возвращает функцию-генератор, которая при каждом вызове выдает очередное значение счетчика i
func intSeq() func() int {
    i := 0
    return func() int {
        i++
        return i
    }
}

//Иногда анонимную функцию передают как аргумент другой функции. Пример из пакета sort : func Search(n int, f func(int) bool) int
a := []int{1, 2, 4, 8, 16, 32, 64, 128}
x := 53
// ближайший сверху к `x` элемент среза `a`
closest := sort.Search(len(a), func(i int) bool { return a[i] >= x }) // 64
```

Указатели
```go
// Указатель (pointer) содержит адрес памяти, который ссылается на конкретное значение.
// Тип *T — указатель на значение типа T. Если указатель не инициализирован, он равен nil
// Оператор & возвращает указатель на значение:
i := 42
iptr = &i
fmt.Println(iptr)
// 0xc000118000

// Оператор * обращается к значению, на которое ссылается указатель. Оно доступно как для чтения, так и для записи:
fmt.Println(*iptr)
// прочитать значение `i` через указатель `iptr`
*iptr = 21
// установить значение `i` через указатель `iptr`

var a, b int
fmt.Scanf("%d-%d", &a, &b)
// Правило работает для скалярных значений (логических, чисел, строк) и массивов. Со срезами и картами другая история.

func Ints(nums []int)
// Срез — это легковесная структура данных, одно из полей которой — указатель на конкретный массив.
// Итого:
// если функция не меняет срез — передавать значение;
// если функция меняет отдельные элементы, но не сам срез — передавать значение;
// если функция меняет сам срез — передавать значение и возвращать новое значение.
```

Структуры
```go
type person struct {
    name string
    age  int
}
bob := person{"Bob", 20}
fmt.Println(bob)
// {Bob 20}

// В Go иногда создают новые структуры через функцию-конструктор с префиксом new:
func newPerson(name string) *person {
    p := person{name: name}
    p.age = 42
    return &p
    //Функция возвращает указатель на локальную переменную — это нормально. Go распознает такие ситуации, и выделяет память под структуру в куче (heap) вместо стека (stack), так что структура продолжит существовать после выхода из функции.
}
// Если функция-конструктор возвращает саму структуру, а не указатель — принято использовать префикс make вместо new:
func makePerson(name string) person {
    p := person{name: name}
    p.age = 42
    return p
}

// Чтобы получить доступ к полям структуры через указатель, не обязательно разыменовывать его через *. Эти два варианта эквивалентны:
sven := &person{name: "Sven", age: 50}
fmt.Println((*sven).age)
fmt.Println(sven.age)
// 50

// Поля структуры можно изменять:
sven.age = 51
fmt.Println(sven.age)
// 51

// Структуры могут включать другие структуры:
type person struct {
    firstName string
    lastName  string
}
type book struct {
    title  string
    author person
}
b := book{
    title: "The Majik Gopher",
    author: person{
        firstName: "Christopher",
        lastName:  "Swanson",
    },
}
fmt.Println(b)
// {The Majik Gopher {Christopher Swanson}}

//можно даже не объявлять отдельный тип:
type user struct {
    name  string
    karma struct {
        value int
        title string
    }
}

u := user{
    name: "Chris",
    karma: struct {
        value int
        title string
    }{
        value: 100,
        title: "^-^",
    },
}
fmt.Printf("%+v\n", u)
// {name:Chris karma:{value:100 title:^-^}}
// Благодаря шаблону %+v, Printf() печатает структуру вместе с названиями полей.

// Поле структуры может ссылаться на другую структуру:
type comment struct {
    text   string
    author *user
}
chris := user{
    name: "Chris",
}
c := comment{
    text:   "Gophers are awesome!",
    author: &chris,
}
fmt.Printf("%+v\n", c)
// {text:Gophers are awesome! author:0xc0000981e0}
```

Методы
```go
type rect struct {
    width, height int
}
func (r rect) area() int {
    return r.width * r.height
}

// Получателем может быть не значение заданного типа, а указатель на это значение:
type circle struct {
    radius int
}
func (c *circle) area() float64 {
    return math.Pi * math.Pow(float64(c.radius), 2)
}
cptr := &circle{radius: 5}
fmt.Println("circle area:", cptr.area())
// circle area: 78.54

//Считается хорошим тоном во всех методах использовать или только значение, или только указатель, но не смешивать одно с другим. 
// Обычно используют указатель: так Go не приходится копировать всю структуру, а метод может ее изменить.
```

Определяемые типы
```go
type inn string
func (id inn) isValid() bool {
    if len(id) != 12 {
        return false
    }
    for _, char := range id {
        if !unicode.IsDigit(char) {
            return false
        }
    }
    return true
}
// Это чем-то похоже на наследование, но механизм более примитивный. Если создать новый определяемый тип на основе inn — он унаследует структуру и свойства inn, но не методы
```

Композиция
```go
// В Go нет наследования. Вместо него активно используется композиция — когда новое поведение собирают из кирпичиков существующего.
// Есть тип «счетчик»:
type counter struct {
    value uint
}
func (c *counter) increment() {
    c.value++
}
func (c *counter) incrementDelta(delta uint) {
    c.value += delta
}

// Мы хотим замерять использование сервисов. Чтобы не дублировать существующие функции, добавим счетчик в тип «использование сервиса»:
type usage struct {
    service string
    counter counter
}
func makeUsage(service string) usage {
    return usage{service, counter{}}
}
//usage.counter.increment()

// Для типа «просмотры страницы» тоже добавим счетчик:
type pageviews struct {
    url *url.URL
    counter counter
}
func makePageviews(uri string) pageviews {
    u, err := url.Parse(uri)
    if err != nil {
        log.Fatal(err)
    }
    return pageviews{u, counter{}}
}
```

Встраивание
```go
type usage struct {
    service string
    counter // встраивание (см пример выше)
}

func makeUsage(service string) usage {
    return usage{service, counter{}}
}
// теперь просто usage.increment()
```

Разное
```go
// Перебор числа по цифрам (Взятие остатка от деления "%" числа на 10, всегда вам даст последнее из его цифр)
number := 123444221
for number > 0 {
  number%10 // Последняя цифра в числе
	number /= 10 // Переходим к следующей
}

strings.Fields(str) - Fields splits the string s around each instance of one or more consecutive white space chars

// используйте math.Pow(x, 2) для возведения в квардрат
// используйте math.Sqrt(x) для извлечения корня
d := math.Sqrt(math.Pow((x1-x2), 2) + math.Pow((y1-y2), 2))

// Составление аббревиатуры
func main() {
	var abbr []rune

	phrase := readString()

	words := strings.Fields(phrase)

	for _, word := range words {
		first := []rune(word)[0]
		if unicode.IsLetter(first) {
			abbr = append(abbr, unicode.ToUpper(first))
		}
	}
	fmt.Println(string(abbr))
}
```
