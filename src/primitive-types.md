# 基本型別

Rust 語言有許多被認為是 "基本" (primitive) 的型別。
這代表他們是內建在語言內的。
Rust 的標準函式庫也提供了許多基於這些基本型別的有用型別，它們也是基本型別。

## 布林 (Booleans)

Rust 內建布林型別，叫做 `bool`。
他有兩種值，`true` 和 `false`：

```rust
let x = true;

let y: bool = false;
```

布林通常用在 [if 條件運算式][if]。

[if]: if.html

你可以在[標準函式庫文件][bool]中找到更多關於 `bool` 的文件。

[bool]: https://doc.rust-lang.org/std/primitive.bool.html

## `char`

`char` 型別代表一個 Unicode 值。
你可以以單引號（`'`）建立 `char`：

```rust
let x = 'x';
let two_hearts = '💕';
```

不像其他語言，這代表 Rust 的 `char` 並非一個位元組，而是四個。

你可以在[標準函式庫文件][char]中找到更多關於 `char` 的文件。

[char]: https://doc.rust-lang.org/std/primitive.char.html

## 數字型別

Rust 有許多數字型別，分為一些不同類型：帶號 (signed) 和非帶號 (unsigned)，固定長度和可變長度，浮點數和整數。

這些型別包含兩部分：類型和大小。
例如，`u16` 是一個 16 位元大小的非帶號型別。
更多位元能讓你有更大的數字。

如果數字在字面上沒有其他東西可以推測他的型別，它會使用預設型別：

```rust
let x = 42; // x has type i32

let y = 1.0; // y has type f64
```

這裡有一份不同數字型別的清單，以及它們在標準函式庫中的文件連結：

* [i8](https://doc.rust-lang.org/std/primitive.i8.html)
* [i16](https://doc.rust-lang.org/std/primitive.i16.html)
* [i32](https://doc.rust-lang.org/std/primitive.i32.html)
* [i64](https://doc.rust-lang.org/std/primitive.i64.html)
* [u8](https://doc.rust-lang.org/std/primitive.u8.html)
* [u16](https://doc.rust-lang.org/std/primitive.u16.html)
* [u32](https://doc.rust-lang.org/std/primitive.u32.html)
* [u64](https://doc.rust-lang.org/std/primitive.u64.html)
* [isize](https://doc.rust-lang.org/std/primitive.isize.html)
* [usize](https://doc.rust-lang.org/std/primitive.usize.html)
* [f32](https://doc.rust-lang.org/std/primitive.f32.html)
* [f64](https://doc.rust-lang.org/std/primitive.f64.html)

讓我們依照分類走一輪吧：

### 帶號 (Signed) 及非帶號 (Unsigned)

整數型別有兩種變形：帶號跟非帶號。
要了解差異之處，讓我們試想有一個 4 位元大小的數字。
如果是帶號整數，4 位元可以讓你儲存 `-8` 到 `+7` 的數字。
代號數字使用 "二補數表示法" (two's complement)。
一個非帶號的 4 位元數字因為不需要儲存負號，所以可以儲存 `0` 到 `+15` 的數字。

非帶號型別使用 `u` 作為分類，而帶號型別使用 `i`。
`i` 代表 "integer"。
所以 `u8` 代表一個 8 位元的非代號數字，而 `i8` 代表 8 位元的帶號數字。

### 固定大小型別

固定大小型別的表現方式有一些特定數量的位元。
有效的位元大小有 `8`、`16`、`32`、`64`。
所以 `u32` 是一個非帶號的 32 位元整數，而 `i64` 是一個帶號的 64 位元整數。

### 可變大小型別

Rust 也有提供一些大小依賴於底層機器的指標大小的型別。
這些型別的分類是 "size"，且分為帶號跟非帶號。
他分為兩類：`isize` 和 `usize`。

### 浮點數型別

Rust 也有兩種浮點數：`f32` 與 `f64`。
他們對應到 IEEE-754 的單精準度和雙精準度浮點數。

## 陣列 (Arrays)

跟很多程式語言一樣，Rust 有用來表現一組事物的清單型別。
最基本的就是 *陣列* (array)，一個固定大小、有相同型別的元素清單。
陣列預設是不可變的 (immutable)。

```rust
let a = [1, 2, 3]; // a: [i32; 3]
let mut m = [1, 2, 3]; // m: [i32; 3]
```

陣列的型別是 `[T; N]`。
我們將在[泛型的章節][generics]提到 `T` 標記。
`N` 是一個編譯期的常數，代表陣列的長度。

有一個可以初始化陣列中所有元素為同一個值的簡寫。
在以下範例，所有 `a` 中的元素將被初始化為 `0`：

```rust
let a = [0; 20]; // a: [i32; 20]
```

你可以透過 `a.len()` 取得 `a` 陣列中元素的個數：

```rust
let a = [1, 2, 3];

println!("a has {} elements", a.len());
```

你也可以使用 *下標* (subscript) 的標記方式存取陣列中特定元素：

> 譯註：此處的 *下標* 應指 `[n]` 這個標記方式。

```rust
let names = ["Graydon", "Brian", "Niko"]; // names: [&str; 3]

println!("The second name is: {}", names[1]);
```

與其他大多數程式語言一樣，下標從零開始，所以第一個元素名稱是 `names[0]`，第二個名稱是 `names[1]`。
上述範例會印出 `The second name is: Brian`。
如果你嘗試使用超出陣列的下標，你會得到錯誤訊息：陣列存取會在執行期做邊界檢查。
在其他系統程式語言中，這種不當存取是許多程式錯誤 (bug) 的根源。

你可以在[標準函式庫文件][array]中找到更多關於 `array` 的文件。

[array]: https://doc.rust-lang.org/std/primitive.array.html

## Slices

一個 "slice" 是一個其他資料結構的參照（或 "視圖"）。
它允許安全、有效的存取陣列的一部份而不用複製陣列。
例如，你可能只想要參考讀到記憶體中的檔案中的某一行。
本質上，一個 "slice" 不是直接建立的，而要從一個已經存在的變數綁定中建立。
Slices 會定義長度，它可以是可變 (mutable) 或不可變 (immutable)。

從內部來看，slices 像是一個指向資料開頭的指標，和它的長度。

### Slicing 語法

你可以使用 `&` 和 `[]` 的組合去從許多資料結構建立 slice。
`&` 說明了 slices 跟[參照][references]很類似，我們會在本節的後面說到細節。
`[]` 帶有範圍的資訊，讓你定義 slice 的長度。

[references]: references-and-borrowing.html

```rust
let a = [0, 1, 2, 3, 4];
let complete = &a[..]; // A slice containing all of the elements in a
let middle = &a[1..4]; // A slice of a: only the elements 1, 2, and 3
```

Slices 的型別是 `&[T]`。
我們將會在[泛型][generics]談到 `T`。

[generics]: generics.html

你可以在[標準函式庫文件][slice]中找到更多關於 slices 的文件。

[slice]: https://doc.rust-lang.org/std/primitive.slice.html

## `str`

Rust 的 `str` 型別是最基本的字串型別。
跟[動態大小型別][dst]，它本身不是很有用，但當它放在參照之後就很有用了，像是 `$str`。
當我們談到[字串][strings]和[參照][references]的時候再來細說。

[dst]: unsized-types.html
[strings]: strings.html
[references]: references-and-borrowing.html

你可以在[標準函式庫文件][str]中找到更多關於 `str` 的文件。

[str]: https://doc.rust-lang.org/std/primitive.str.html

## 多元組 (Tuples)

一個多元組 (tuple) 是一組固定大小的有序 (ordered) 清單。
就像：

```rust
let x = (1, "hello");
```

這是長度為 2 的多元組，由括弧和逗號組成。
以下是同樣的程式碼，但是多了型別註釋：

```rust
let x: (i32, &str) = (1, "hello");
```

如你所見，多元組的型別與多元組很像，只是各個位置是型別名稱而不是數值。
心細的讀者會注意到多元組是異質的 (heterogeneous)：我們在一個多元組中同時有 `i32` 與 `&str`。
在系統程式語言中，字串會比其他語言複雜一點。
現在，把 `&str` 當作 *字串 slice*，我們很快就會說到。

當兩個多元組有相同的型別和[元數][arity] (arity) 時，你可以把一個多元組賦值給另一個。
相同長度的多元組有著相同的元數。

[arity]: glossary.html#元數%20(Arity)

```rust
let mut x = (1, 2); // x: (i32, i32)
let y = (2, 3); // y: (i32, i32)

x = y;
```

你可以透過 *destructuring let* 存取多元組的欄位。
以下是範例：

```rust
let (x, y, z) = (1, 2, 3);

println!("x is {}", x);
```

還記得[之前][let]當我提到 `let` 陳述式的左邊遠比賦值綁定還強大嗎？
這就是了。
我們可以放一個模式 (pattern) 在 `let` 的左邊，然後如果它跟右邊相符，我們就可以一次賦值多個綁定。
在這情形下，`let` "解構" 或 "拆開" 了多元組，然後賦值到三個綁定上。

[let]: variable-bindings.html

這樣的模式非常強大，我們將會在後面一直看到。

在括號中只有一個值時，在值後面加上逗號，可以消歧義確定它是一個單元素的多元組：

```rust
(0,); // single-element tuple
(0); // zero in parentheses
```

### 多元組索引 (Tuple Indexing)

你也可以用索引語法去存取多元組的欄位：


```rust
let tuple = (1, 2, 3);

let x = tuple.0;
let y = tuple.1;
let z = tuple.2;

println!("x is {}", x);
```

就像陣列的索引，它從零開始，但是不像陣列所以，它使用 `.` 而不是 `[]`。

你可以在[標準函式庫文件][tuple]中找到更多關於多元組的文件。

[tuple]: https://doc.rust-lang.org/std/primitive.tuple.html

## 函式

函式也有型別！
它們像這樣：

```rust
fn foo(x: i32) -> i32 { x }

let x: fn(i32) -> i32 = foo;
```

在這個例子中，`x` 是 "函式指標" 指向一個有 `i32` 參數並回傳 `i32` 的函式。


> *commit 228afd7*
