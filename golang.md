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
// Цикл от 0 до n-1 (Go 1.22+):
const n = 10
for i := range n {
    fmt.Print(i, " ")
}
// Если хотим повторить определенное действие n раз, а конкретное значение счетчика цикла не интересно (Go 1.22+):
const n = 10
for range n {
    fmt.Print(".")
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

Интерфейсы
```go
type geometry interface {
    area() float64
    perim() float64
}
// реализация
type rect struct {
    width, height float64
}
func (r rect) area() float64 {
    return r.width * r.height
}
func (r rect) perim() float64 {
    return 2*r.width + 2*r.height
}

// Встраивание интерфейса
// Есть тип «счетчик»:
type counter struct {
    val uint
}
func (c *counter) increment() {
    c.val++
}
func (c *counter) value() uint {
    return c.val
}
// Мы хотим встраивать счетчик в другие типы, но не давать прямой доступ к полю val — чтобы менять значение счетчика можно было только через методы.
Определим интерфейс счетчика:
type Counter interface {
    increment()
    value() uint
}
// И вместо конкретного типа counter встроим интерфейс Counter, который он реализует:
type usage struct {
    service string
    Counter
}
// В конструкторе будем создавать конкретное значение типа counter, но потребителям об этом знать не обязательно:
func newUsage(service string) *usage {
    return &usage{service, &counter{}}
}

// Пустой интерфейс
interface{}
// Пустой интерфейс может содержать значение любого типа (ведь у каждого типа есть как минимум 0 методов). Пустые интерфейсы используют, если тип значения заранее не известен. 
// Например, функция из пакета fmt:
func Println(a ...interface{}) (n int, err error)

// Приведение типа
// Приведение типа (type assertion) извлекает конкретное значение из переменной интерфейсного типа:
var ival interface{} = "hello"

str, ok := ival.(string)
fmt.Println(str, ok)
// hello true
flo, ok = ival.(float64)
fmt.Println(flo, ok)
// 0 false

// Переключатель типа
switch ival.(type) {
case string:
	fmt.Println("It's a string")
case float64:
	fmt.Println("It's a float")
default:
	fmt.Println("It's a mystery")
}
// It's a string
```

Ошибки
```go
func sqrt(x float64) (float64, error) {
    if x < 0 {
        return 0, errors.New("expect x >= 0")
    }
    // `nil` в качестве ошибки указывает, что ошибок не было.
    return math.Sqrt(x), nil
}

for _, x := range []float64{49, -49} {
    if res, err := sqrt(x); err != nil {
        fmt.Printf("sqrt(%v) failed: %v\n", x, err)
    } else {
        fmt.Printf("sqrt(%v) = %v\n", x, res)
    }
}
// sqrt(49) = 7
// sqrt(-49) failed: expect x >= 0

// Собственный тип ошибки (Чтобы создать собственный тип ошибки, достаточно реализовать метод Error().)
type error interface {
    Error() string
}

type lookupError struct {
    src    string
    substr string
}
func (e *lookupError) Error() string {
    return fmt.Sprintf("'%s' not found in '%s'", e.substr, e.src)
}

func indexOf(src string, substr string) (int, error) {
    idx := strings.Index(src, substr)
    if idx == -1 {
        // Создаем и возвращаем ошибку типа `lookupError`.
        return -1, &lookupError{src, substr}
    }
    return idx, nil
}

// Проверим работу indexOf() для разных подстрок.
src := "go is awesome"
for _, substr := range []string{"go", "js"} {
    if res, err := indexOf(src, substr); err != nil {
        fmt.Printf("indexOf(%#v, %#v) failed: %v\n", src, substr, err)
    } else {
        fmt.Printf("indexOf(%#v, %#v) = %v\n", src, substr, res)
    }
}
// indexOf("go is awesome", "go") = 0
// indexOf("go is awesome", "js") failed: 'js' not found in 'go is awesome'

// Поскольку indexOf() возвращает общий тип error, чтобы получить доступ к конкретному объекту ошибки, придется использовать приведение типа:
_, err := indexOf(src, "js")
if err, ok := err.(*lookupError); ok {
    fmt.Println("err.src:", err.src)
    fmt.Println("err.substr:", err.substr)
}
// err.src: go is awesome
// err.substr: js
```

Defer
```go
func main() {
    f, err := createFile("/tmp/defer.txt")
    if err != nil {
        log.Fatal("Error creating file: ", err)
    }
    defer closeFile(f) // Закрыть по выходу из функции
    if err := writeFile(f); err != nil {
        log.Fatal("Error writing to file: ", err)
    }
}
```

Panic / Recover
```go
// Если во время выполнения программы происходит неисправимая ошибка, срабатывает паника (panic). Это аналог исключения
// Допустим, мы написали функцию, которая возвращает символ строки по индексу, но забыли проверить, что индекс попадает в границы:
func getChar(str string, idx int) byte {
    return str[idx]
}
// Если вызвать getChar() с некорректным индексом — сработает паника:
c := getChar("hello", 10)
// panic: runtime error: index out of range [10] with length 5

//Панику можно вызвать и вручную с помощью одноименной встроенной функции:
panic("oops")
// Так редко делают — в Go принято возвращать ошибку из функции, а не паниковать.

// Recover
// Отловить любые непредвиденные ошибки:

func protect(fn func()) {
    defer func() {
        if err := recover(); err != nil {
            fmt.Println("ERROR:", err)
        } else {
            fmt.Println("Everything went smoothly!")
        }
    }()
    fn()
}

//Здесь сработает паника:
protect(func() {
    c := getChar("hello", 10)
    fmt.Println("hello[10] = ", c)
})
// ERROR: runtime error: index out of range [10] with length 5

// А здесь функция отработает без ошибок:
protect(func() {
    c := getChar("hello", 4)
    fmt.Println("hello[4] =", c)
})
// hello[4] = 111
// Everything went smoothly!
```

Новый модуль
```sh
# Модуль инициализируется командой go mod init:

$ mkdir match
$ cd match
$ go mod init github.com/averveiko/match
go: creating new go.mod: module github.com/averveiko/match
$ touch main.go

# go.mod описывает модуль: идентификатор, минимальная версия Go, зависимости (их пока нет)
# Если программа компилируется в исполняемый файл, у нее должен быть пакет с названием main. Если бы мы делали библиотеку — назвали бы файл match.go, а пакет match.
```

Сборка и установка
```sh
go build # собирает модуль в исполняемый файл
go install # устанавливает модуль в домашний каталог Go ($HOME/go для Linux/macOS, %USERPROFILE%\go для Windows):
```

Внешние зависимости
```sh
$ go get github.com/sahilm/fuzzy
go: downloading github.com/sahilm/fuzzy v0.1.0
go get: added github.com/sahilm/fuzzy v0.1.0
# go get помещает скачанный модуль в домашний каталог Go (~/go). В каталог с исходниками нашего модуля он не попадает. Таким образом, все внешние зависимости хранятся в общей куче, а не в конкретных проектах (в противоположность venv в питоне и node_modules в js).
```
```go
// Подключим и используем в main.go:
import (
    // ...
    "github.com/sahilm/fuzzy"
)
func main() {
    // ...
    matches := fuzzy.Find(pattern, []string{src})
    isMatch := len(matches) > 0
    // ...
}

//Зависимость зафиксирована в go.mod:
module github.com/gothanks/match

go 1.16

require (
    github.com/kylelemons/godebug v1.1.0 // indirect
    github.com/sahilm/fuzzy v0.1.0
)
// Модуль github.com/kylelemons/godebug — транзитивная зависимость (от него зависит github.com/sahilm/fuzzy), поэтому записан как indirect.
```

Обновление зависимостей
```sh
# Показать основной модуль и его зависимости:
$ go list -m all
github.com/gothanks/match
github.com/kylelemons/godebug v1.1.0
github.com/sahilm/fuzzy v0.1.0

# Посмотреть все версии конкретного модуля:
$ go list -m -versions github.com/sahilm/fuzzy
github.com/sahilm/fuzzy v0.0.1 v0.0.2 v0.0.3 v0.0.4 v0.0.5 v0.1.0

# Обновить конкретный модуль на последнюю patch-версию в пределах действующей minor-версии (например, 1.1.0 → 1.1.5):
$ go get -u=patch github.com/sahilm/fuzzy

# Обновить конкретный модуль на последнюю minor-версию в пределах действующей major-версии (1.1.0 → 1.2.1):
$ go get -u github.com/sahilm/fuzzy

# Go предполагает, что авторы модулей следуют правилам семантического версионирования, при котором minor- и patch-версии остаются обратно совместимыми — поэтому go get -u обновляет на последнюю выпущенную версию.
# Если меняется major-версия (например, 1.2.1 → 2.0.0), у модуля меняется идентификатор (github.com/sahilm/fuzzy → github.com/sahilm/fuzzy/v2). go get -u автоматически на такую версию не обновит. Это сделано специально, потому что новая major-версия может быть несовместима с предыдущей.

# Удалить неактуальные зависимости из go.mod:
$ go mod tidy
```

Тесты см. https://github.com/averveiko/wordcount
Тесты принято писать в отдельном файле с суффиксом _test. Например, если IntMin() определена в файле ints.go, то тесты будут в том же пакете, но в отдельном файле ints_test.go.
Тест — это функция с префиксом Test:
```go
func TestIntMin(t *testing.T) {
    got := IntMin(2, -2)
    want := -2
    if got != want {
        t.Errorf("got %d; want %d", got, want)
    }
}
```
Тест всегда принимает указатель на testing.T — это такой сборник полезных функций. Например, t.Errorf() выводит сообщение об ошибке, не прекращая выполнение теста.
Выполним тест с подробными результатами (опция -v):
```sh
$ go test -v
=== RUN   TestIntMin
--- PASS: TestIntMin (0.00s)
PASS
ok      ints  0.004s
```

Тестовое покрытие
Покрытие (coverage) показывает, насколько полно тесты проверяют основной код. Чтобы его замерить, достаточно запустить go test с опцией -cover.
```sh
# Запускаем тесты:
$ go test -cover
PASS
coverage: 66.7% of statements
ok      ints    0.004s

#Только 66%! Чтобы не гадать, какой код не покрыт, попросим Go собрать детальную статистику (profile):
$ go test -coverprofile=coverage
PASS
coverage: 66.7% of statements
ok      ints    0.004s

#И посмотрим отчет в HTML:
go tool cover -html=coverage
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
