#HSLIDE
## Основи на Elixir : Типове и pattern matching
![Image-Absolute](assets/basics.jpg)

#HSLIDE
## За какво ще си говорим днес?

1. Интерпретатор на езика
2. Основни типове
3. Съпоставяне на образци
4. Неизменимост

#HSLIDE
## Интерпретатор на езика
![Image-Absolute](assets/terminal.png)

#HSLIDE
### Изисквания за курса

* `Erlang/OTP >= 19`
* `Elixir >= 1.4`
* Удобен за вас редактор или IDE
* Основни познания по `GIT` и инсталиран `GIT`

#HSLIDE
### Какво е IEx?
* IEx е съкращение от 'Interactive Elixir' и е интерпретатор на езика.
* Всяка команда която изпълните в `iex` се интерпретира и стойността ѝ се отпечатва.

#HSLIDE
```elixir
iex(1)> 1 + 1
2
```

#HSLIDE
### BREAK меню
Как да излезем от `iex`?
При натискане на `CTRL-c` няма да излезете от `iex`. Ще ви излезе меню.

#HSLIDE
### User Switch меню
* Друго интересно меню е `User Switch` менюто.
* В него можем да влезем с `CTRL-g`.

#HSLIDE
Стартиране на друга `iex` сесия:
```elixir
User switch command
--> s 'Elixir.IEx'
--> c

```

#HSLIDE
* Това ще инициализира нова `iex` сесия.
* Тя ще е напълно изолирана от предишната.
* Можем да се върнем към първоначалната сесия с `CTRL-g` и `c 1`.
* С `j` в `User Switch` менюто виждаме списък от сесиите, както и коя е активна в момента.

#HSLIDE
### Отдалечени сесии
* `IEx` може да се свърже с вече съществуваща `iex` сесия.
* Това е възможно, даже ако сесията е на друга машина.
* Има едно условие. Сесиите към които се свързваме трябва да са именовани.

#HSLIDE
Можем да пуснем именована `iex` сесия с:
```elixir
iex --sname one
iex(one@meddland)1> node()
:one@meddland
```

#HSLIDE
Нека в друг терминал да стартираме друга сесия.
```elixir
iex --sname two
iex(two@meddland)1> node()
:two@meddland
```

#HSLIDE
И сега нека от сесия `two` да се свържем към сесия `one` с `CTRL-g`:
```elixir
User switch command
--> r 'one@meddland' 'Elixir.IEx'
--> c

Interactive Elixir (1.4.1) - press Ctrl+C to exit (type h() ENTER for help)
iex(one@meddland)1>
```

#HSLIDE
### Важни функции за ползване в `iex`
Ако в `iex` напишем просто `h` и го изпълним ще видим списък с функции, които
можем да ползваме. Ето някои от тях:

#HSLIDE
За компилиране - `c`.
```elixir
iex(1)> c "path/to/file.ex"
```
По този начим можем да тестваме през `iex` функционалност, която сме записали във
файл.

#HSLIDE
Друга интересна функция е `i`.
```elixir
iex(1)> i 1
Term
  1
Data type
  Integer
Reference modules
  Integer

```
Отпечатва информация за типа на аргумента си.

#HSLIDE
Функцията `h` може да приема функция.
Това ще отпечата документацията на функцията.
```elixir
iex(1)> h is_integer

def is_integer(term)

Returns true if term is an integer; otherwise returns false.

Allowed in guard tests. Inlined by the compiler.
```

#HSLIDE
## Основни типове
![Image-Absolute](assets/types.png)

#HSLIDE
### Числа
`Elixir` предлага цели числа и числа с плаваща запетая:
```elixir
iex> 1 # В десетична бройна система
1
iex>10_000
10000
iex> 0x53 # В шестнадесетична
83
iex> 0o53  # В осмична
43
iex> 0b11 # В двоична
3
iex> 3.14 # С плаваща запетая
3.14
iex> 1.0e-10 # С плаваща запетая
1.0e-10
```

#HSLIDE
```elixir
iex> 1 + 41
42
iex> 21 * 2
42
iex> 54 / 6 # Връща резултат с плаваща запетая
9.0
iex> div(54, 6) # В повечето езици `/` прави това
9
iex> rem 11, 3 # А ето как получаваме остатъка.
2
```

#HSLIDE
```elixir
iex> 1 < 2
true
iex> 1 <= 2
true
iex> 1 >= 1
true
iex> 1 > 1
false
iex> 1 != 2
true
iex> 1 == 2
false
iex> 1 == 1.0 # Операторът == сравнява по стойност
true
iex> 1 === 1.0 # Операторът === сравнява по стойност И тип
false
iex> 1 !== 1.0 # Операторът !== сравнява по стойност И тип
true
```

#HSLIDE
### Булеви стойности: true/false
```elixir
iex> true
true
iex> false
false
iex> is_boolean(true)
true
iex> is_boolean(0)
false
iex> true == false
false
```

#HSLIDE
### Атоми
* Атомите са константи, чието име е стойността им.
* Булевите стойности `true` и `false` всъщност са атомите `:true` и `:false`
* Имената на модули (колекции от функции и нещо повече) в `Elixir` също са атоми.
* Модули идващи от `Erlang` са реферирани от атоми.
* GC не ги събира.

#HSLIDE
### Атоми
* Удобни са за ползване като ключове в `map`-ове.
* Задължителна част от `keyword lists`.
* Често се използват в кортежи за означаване на резултат от функция. Пример - ``{:ok, 2}``
* Освен ако не са в двойни кавички, атомите могат да съдържат подчертавки, цифри и латински букви, както и at(`@`).
* Атомите могат да завършват на `!` или на `?`.
* Идеални за pattern matching (съпоставянето им е еквивалентно на съпоставяне на числа)

#HSLIDE
```elixir
iex> :atom
:atom
iex> :true
true
iex> :anoter_atom
:anoter_atom
iex> SomeModule # Може и да не е дефиниран
SomeModule
iex> is_atom(:atom)
true
iex> is_atom(true)
true
iex> true == :true
true
iex> :"atom with a space" # Могат да се дефинират и така
:"atom with a space"
```

#HSLIDE
### Низове
Низовете в `Elixir` се дефинират с двойни кавички и са с _UTF-8_ encoding:

#HSLIDE
```elixir
iex> "Здрасти"
"Здрасти"
iex> "Здрасти #{:Pesho}" # Интерполация
"Здрасти Pesho"
iex> "Един
...> стринг
...> на
...> повече
...> от един ред"
"Един\nстринг\nна\nповече\nот един ред" # Поддръжа на множество редове
iex> is_binary("Здрасти") # Низовете представляват поредица от байтове
true
iex> String.length("Здрасти") # Брой на символи
7
iex> byte_size("Здрасти") # Брой на байтове
14
iex> "Бял" <> " мерцедес!" # Конкатенация
"Бял мерцедес!"
```

#HSLIDE
### Списъци
* Има специален модул, `List`, за работа с тях.
* Не държат стойностите си подредени в паметта.
* Намирането на дължината им, четене на стойност по index, добавяне на стойност на index и триене на стойност на index са все линейни операции.

#HSLIDE
```elixir
iex> [1, 2, "три", 4.0] # Не са хомогенни
[1, 2, "три", 4.0]
iex> length [1, 2, 3, 5, 8] # Дължината
5
iex> hd [1, 2, 3, 5, 8] # Връща първия елемент (head)
1
iex> tl [1, 2, 3, 5, 8] # Връща списък с елементите без първия (tail)
[2, 3, 5, 8]
iex> is_list([1, 2])
true
iex> 'Еликсир' # Единични кавички - списък от unicode codepoint-и
[1045, 1083, 1080, 1082, 1089, 1080, 1088]
iex> [83, 79, 83]
'SOS'
```

#HSLIDE
### Кортежи
![Image-Absolute](assets/tuple.jpg)

#HSLIDE
### Кортежи
* Кортежите съхраняват елементите си подредени един след друг в паметта.
* Достъпът до елемент по индекс и взимането на дължината им са константни операции.
* Ползват се за много неща:
  * Заедно с атомите за връщане на множество стойности от функция.
  * За `pattern matching` - ще видим малко по-долу.
  * Read-only колекция, защото писането в тях е скъпа операция.

#HSLIDE
```elixir
iex> {:ok, 7}
{:ok, 7}
iex> tuple_size({:ok, 7, 5})
3
iex> is_tuple({:ok, 7, 5})
true
```

#HSLIDE
### Keyword lists
Списъци, които съдържат `tuple`-и от по два елемента - атом и каквато и да е стойност.

#HSLIDE
```elixir
iex>[{:one, 1}, {:two, 2}]
[one: 1, two: 2]  # Както виждате има специален синтаксис за тях. Това е същото:
iex> [one: 1, two: 2]
[one: 1, two: 2]
```

#HSLIDE
* Ако keyword list е последен аргумент на функция, можем да пропуснем квадратните скоби при извикване:
```elixir
iex> f(1, 2, three: 3, four: 4)
```

#HSLIDE
* Ключовете им могат да се повтарят.
* Използват се и за предаване на command line параметри или опции на функции.
* Пример е `String.split/3`.
```elixir
iex> String.split("one,two,,,three,,,four", ",", trim: true)
["one", "two", "three", "four"] # Няма празни низове заради опцията trim: true.
```

#HSLIDE
### Maps
* Колекции от ключове и стойности.
* `Map`-овете в `Elixir` не позволяват еднакви ключове.
* За ключове може да се използва всичко и дори няма нужда да бъдат един и същи тип,
но обикновено се използват низове или атоми.

#HSLIDE
### Бинарен тип (Binaries)
Прдставляват поредици от битове и байтове.
```elixir
iex> << 2 >> # Цялото число 2 в 1 байт
<<2>>
iex> byte_size << 2 >>
1
iex> << 255 >> # Цялото число 255 в 1 байт
<<255>>
iex> << 256 >> # Превърта и става 0
<<0>>
iex> <<1, 2>> # Две цели числа в два байта.
<<1, 2>>
iex> byte_size << 1, 2 >>
2
```

#HSLIDE
```elixir
iex> << 5::size(3), 1::size(1), 5::size(4) >>
<<181>>
iex> 0b10110101
181
iex> byte_size << 5::size(3), 1::size(1), 5::size(4) >>
1
iex> is_bitstring << 5::size(3), 1::size(1) >>
true
iex> is_binary << 5::size(3), 1::size(1) >>
false
```

#HSLIDE
* Интересн факт - низовете в `Elixir` са имплементирани като `binary` тип.
* Спомняте си че `is_binary("Стринг")` връщаше `true`.
```elixir
iex> <<208, 170, 208, 156>> = "ЪМ"
"ЪМ"
```

#HSLIDE
### Анонимни функции
```elixir
iex> fn (x) -> x + 1 end
#Function<6.52032458/1 in :erl_eval.expr/5>
iex> (fn (x) -> x + 1 end).(4) # Извикване
5
iex> is_function((fn (x) -> x + 1 end))
true
```

#HSLIDE
```elixir
iex> &(&1 + 1)
#Function<6.52032458/1 in :erl_eval.expr/5>
iex> (&(&1 + 1)).(4)
5
```

#HSLIDE
### Други типове
* Други типове са `Port`, `Reference` и `PID`, които се използват с процеси.
* Има и регулярни изрази. `~r/\w+/im`
* Ranges : `(1..1000)`
* Има различни shortcut-синтаксиси за дефиниране на някои от типовете.

#HSLIDE
## Съпоставяне на образци
![Image-Absolute](assets/patterns.jpg)

#HSLIDE
## Съпоставяне на образци
* В Elixir `pattern matching`-a е еднa от най-важните и основни особености.
* Операторът `=` се нарича `match operator`.
* Можем да го сравним с знака `=` в математиката.
* Използвайки го, превръщаме целия израз в уравнение, в което сравняваме лявата с дясната страна.
* Ако сравнението е успешно се връща стойността на това уравнение, ако не - има грешка.

#HSLIDE
```elixir
iex> x = 5
5
iex> 5 = x
5
iex> 4 = x
** (MatchError) no match of right hand side value: 1

```

#HSLIDE
#### Засега за `match operator`-а знаем:
1. С него могат да се дефинират променливи.
2. С него могат да се правят проверки - дали дадена променлива има дадена стойност.

#HSLIDE
* Имената на променливи задължително започват с малка латинска буква или подчертавка (`_`),
следвана от букви, цифри или подчертавки.
* Могат да завършват на `?` или `!`.
* Операторът `=` ще опита да присвои на всички възможни променливи от ляво стойности от дясно.

#HSLIDE
```elixir
iex> {one, tWo, t3, f_our, five!} = {1, 2, 3, 4, 5}
{1, 2, 3, 4, 5}
iex> one
1
iex> tWo
2
iex> t3
3
iex> f_our
4
iex> five!
5
```

#HSLIDE
```elixir
iex(107)> [head|tail] = [1, 2, 4, 5]
[1, 2, 4, 5]
iex> head
1
iex> tail
[2, 4, 5]
iex> [a, b|tail] = [1, 2, 4, 5]
[1, 2, 4, 5]
iex> a
1
iex> b
2
iex> tail
[4, 5]
```

#HSLIDE
```elixir
iex> g = fn
...>   0 -> 0
...>   x -> x - 1
...> end
#Function<6.52032458/1 in :erl_eval.expr/5>
iex> g.(0)
0
iex> g.(3)
2
```

#HSLIDE
![Image-Absolute](assets/pins.jpg)

#HSLIDE
* В `Elixir` e възможно да променим стойността на променлива.
* В `Erlang` това не е възможно.

#HSLIDE
Ако искаме една променлива, която вече съществува да не промени стойността си при съпоставяне,
можем да използваме `pin` оператора - `^`.
```elixir
iex> x = 5
5
iex> ^x = 6
** (MatchError) no match of right hand side value: 6
```

#HSLIDE
```elixir
iex> {y, ^x} = {5, 4}
```

#HSLIDE
Ако се опитаме да присвоим стойност на `unbound` променлива (досега не е съществувала),
използвайки `pin` оператора, ще получим грешка.
```elixir
iex> ^z = 4
** (CompileError) iex:56: unbound variable ^z
```

#HSLIDE
## Неизменимост
![Image-Absolute](assets/immutable.png)

#HSLIDE
## Неизменимост
* Ще си говорим за immutability, pattern matching и рекурсия на всяка лекция.
* Или поне ще ги споменаваме. <!-- .element: class="fragment" -->
* Както и за процесите! <!-- .element: class="fragment" -->

#HSLIDE
```elixir
iex> base_list = [1, 2, 3]
[1, 2, 3]
iex> new_list = [0 | base_list]
[0, 1, 2, 3]
```

#HSLIDE
## Това беше за днес.
![Image-Absolute](assets/end.jpg)
