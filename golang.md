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
