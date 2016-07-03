---
layout: page
title: Basics
category: basics
order: 1
lang: th
---

เรามาเริ่มต้นกันด้วย ชนิดของข้อมูล และ ปฏิบัติการพื้นฐาน

{% include toc.html %}

## ก่อนอื่นเลย...

### ติดตั้ง Elixir

วิธีติดตั้งสำหรับระบบปฏิบัติการต่างๆ ไปหาอ่านได้ที่ elixir-lang.org ตรง [วิธีติดตั้ง Elixir](http://elixir-lang.org/install.html)

หลังจากที่ติดตั้ง Elixir แล้ว นายสามารถตรวจสอบเวอร์ชั่นที่ติดตั้งได้ดังนี้

    % elixir -v
    Erlang/OTP {{ site.erlang.OTP }} [erts-{{ site.erlang.erts }}] [source] [64-bit] [smp:4:4] [async-threads:10] [hipe] [kernel-poll:false] [dtrace]

    Elixir {{ site.elixir.version }}

### ทดลองโหมด Interactive

Elixir มาพร้อมกับ  `iex` ซึ่งเป็น interactive shell ที่เราสามารถใช้สำหรับทดสอบ expressions ต่างๆ ได้ขณะเขียนโปรแกรม

เริ่มด้วยกันรันคำสั่ง `iex` บน shell:

    Erlang/OTP {{ site.erlang.OTP }} [erts-{{ site.erlang.erts }}] [source] [64-bit] [smp:4:4] [async-threads:10] [hipe] [kernel-poll:false] [dtrace]

    Interactive Elixir ({{ site.elixir.version }}) - press Ctrl+C to exit (type h() ENTER for help)
    iex(1)>
   
จากนั้นให้นายเริ่มทดลองโดยการพิมพ์ expressions ง่ายๆ สักสองสามอัน:

    iex(1)> 2+3
    5
    iex(2)> 2+3 == 5
    true
    iex(3)> String.length("The quick brown fox jumps over the lazy dog")
    43

อย่าได้ห่วงไปหากนายไม่เข้าใจทุกอย่างตอนนี้ แต่หวังว่านายจะเก็ทไอเดีย

## ชนิดข้อมูลพื้นฐาน

### เลขจำนวนเต็ม (Integers)

```elixir
iex> 255
255
iex> 0xFF
255
```

Elixir มาพร้อมสรรพกับ support สำหรับ เลขฐานสอง,เลขฐานแปด และเลขฐานสิบหก:

```elixir
iex> 0b0110
6
iex> 0o644
420
iex> 0x1F
31
```

### เลขทศนิยม (Floats)

ในภาษา Elixir นายสามารถแทนที่เลขทศนิยมด้วยการใส่ทศนิยมอย่างน้อยหนึ่งหลัก เลขทศนิยมมีความแม่นยำแบบ double precision และ support `e` สำหรับเลขยกกำลัง:

```elixir
iex> 3.41
3.41
iex> .41
** (SyntaxError) iex:2: syntax error before: '.'
iex> 1.0e-10
1.0e-10
```


### บูลีน (Booleans)

ใน Elixir ตรรกะบูลีนแทนที่ด้วย `true` กับ `false` ทุกอย่างถือว่าเป็นจริงหมด ยกเว้น `false` และ `nil`:

```elixir
iex> true
true
iex> false
false
```

### อะตอม (Atoms)

อะตอม คือ ตัวแปรคงที่ที่ตัวมันเองเป็นทั้งชื่อตัวแปรและค่าของตัวแปร ถ้านายเคยผ่าน Ruby มาก่อน อะตอมในที่นี่เหมือนกับ Symbols ใน Ruby

```elixir
iex> :foo
:foo
iex> :foo == :bar
false
```

ค่าบูลีน `true` และ `false` จริงๆ เป็น อะตอม  `:true` และ `:false` ไม่เชื่อลองดู

```elixir
iex> true |> is_atom
true
iex> :true |> is_boolean
true
iex> :true === true
true
```

ชื่อของ modules ใน Elixir ก็เป็นอะตอมเหมือนกัน `MyApp.MyModule` เป็นอะตอมที่สมบูรณ์ แม้ว่านายจะยังไม่ได้สร้าง module ดังกล่าวก็ตาม

```elixir
iex> is_atom(MyApp.MyModule)
true
```

นายยังสามารถใช้อะตอมในการอ้างอิง modules จาก library ในภาษา Erlang รวมถึง built in modules ได้ด้วย

```elixir
iex> :crypto.rand_bytes 3
<<23, 104, 108>>
```

### สตริง (Strings)

สตริงใน Elixir นั้น UTF-8 encoded (ใช้แทนภาษาสากลได้ เช่นภาษาไทย) และล้อมด้วยเครื่องหมายคำพูด:

```elixir
iex> "Hello"
"Hello"
iex> "dziękuję"
"dziękuję"
```

นายสามารถใช้เครื่องหมายขึ้นบรรทัดใหม่ (line breaks) กับ escape sequences กับสตริงได้เหมือนภาษาอื่นๆ:

```elixir
iex> "foo
...> bar"
"foo\nbar"
iex> "foo\nbar"
"foo\nbar"
```

Elixir ยังมีชนิดข้อมูลที่ซับซ้อนมากว่านี้ เราจะค่อยๆ เรียนไปเมื่อเราถึงบทเกี่ยวกับ Collections และ Functions

## ปฏิบัติการพื้นฐาน

### พีชคณิต

Elixir สนับสนุนเครื่องหมายปฏิบัติการทางคณิตพื้นฐาน เช่น `+`, `-`, `*`, และ `/` ตามที่นายคาด จุดสำคัญที่ต้องจำ คือ `/` จะ return ค่าทศนิยมเสมอ:

```elixir
iex> 2 + 2
4
iex> 2 - 1
1
iex> 2 * 5
10
iex> 10 / 5
2.0
```

ถ้านายต้องการหารเลขจำนวนเต็ม หรือหาค่าเศษของการหาร ให้ใช้ functions ดังนี้:

```elixir
iex> div(10, 5)
2
iex> rem(10, 3)
1
```

### บูลีน (Booleans)

Elixir มีเครื่องหมายปฏิบัติการทางตรรกะ `||`, `&&`, และ `!` เครื่องหมายดังกล่าวสามารถใช้กับข้อมูลชนิดใดก็ได้: 

```elixir
iex> -20 || true
-20
iex> false || 42
42

iex> 42 && true
true
iex> 42 && nil
nil

iex> !42
false
iex> !false
true
```

มีเครื่องหมายทางตรรกะสามตัวที่บังคับว่า ค่าแรกจะต้องเป็นบูลีนเท่านั้น:

```elixir
iex> true and 42
42
iex> false or true
true
iex> not false
true
iex> 42 and true
** (ArgumentError) argument error: 42
iex> not 42
** (ArgumentError) argument error
```

### การเปรียบเทียบ

Elixir มาพร้อมกับเครื่องหมายสำหรับเปรียบเทียบค่าต่างๆ ที่เราคุ้นเคย: `==`, `!=`, `===`, `!==`, `<=`, `>=`, `<` และ `>`.

```elixir
iex> 1 > 2
false
iex> 1 != 2
true
iex> 2 == 2
true
iex> 2 <= 3
true
```

สำหรับการเปรียบเทียบแบบเข้มงวด (เทียบชนิดด้วย) ระหว่าง จำนวนเต็ม และทศนิยม ให้ใช้ `===`:

```elixir
iex> 2 == 2.0
true
iex> 2 === 2.0
false
```
คุณลักษณะที่สำหรับของ Elixir คือ ชนิดข้อมูลสองชนิดใดๆ ก็ตามสามารถเปรียบเทียบกันได้ ซึ่งมีประโยชน์ในการจัดลำดับ (sorting) เราไม่จำเป็นต้องจำลำดับนี้ และอย่างน้อยให้รู้ว่ามีลำดับนี้อยู่:

```elixir
number < atom < reference < functions < port < pid < tuple < maps < list < bitstring
```

คุณลักษณะนี้สามารถนำไปสู่การเปรียบเทียบที่น่าสนใจที่นายอาจไม่พบเจอในภาษาอื่นๆ:

```elixir
iex> :hello > 999
true
iex> {:hello, :world} > [1, 2, 3]
false
```

### การแทนที่สตริง (String Interpolation)

หากนายเคยใช้ Ruby การแทนที่สตริงใน Elixir จะดูคุ้นตามาก:

```elixir
iex> name = "Sean"
iex> "Hello #{name}"
"Hello Sean"
```

### การต่อสตริง (String Concatenation)

การต่อสตริงสามารถทำได้ด้วยกันใช้เครื่องหมาย `<>`:

```elixir
iex> name = "Sean"
iex> "Hello " <> name
"Hello Sean"
```
