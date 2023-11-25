# Guessing_game

#### 为了获取用户的输入输出，需要使用 io 输入输出库

```rust
use std::io;
```

io 这个库来自于 std，所以从 std 中导入

rust 程序中如果使用的类型不在范围中，必须使用 `use` 语句显示的将该类型引入范围

`std::io` 库提供很多有用的功能，其中就包括接受用户的输入输出

#### `main` 函数时程序的入口

`fn` 语法声明一个新函数

`println!` 是一个将字符串打印到屏幕上的宏

#### 可以使用 `let` 创建一个新变量，`Rust` 中默认变量是不可变的，意味着一旦我们给变量赋于了值，该值就不会改变。

为了使变量可变，在变量名前添加 `mut` 

```rust
let apple = 5; // immutable
let mut bananas = 5; // mutable
```

`//` 注释语法

#### `String` 是标准库提供的字符串类型，它是可增长的 `UTF-8` 编码文本位。

```rust
let mut guess = String::new(); // 创建了一个可变变量，该变量当前绑定到 String 的新空实例
```

`::new()` 中的 `::` 语法表示 `new` 是 `String` 类型的关联函数。关联函数是在类型上实现的函数，在此为 `String` 。此 `new` 函数创建一个新的字符串。`new` 是生成某种新值的函数的通用名称。

#### 从 `io` 模块中调用 `stdin` 函数了，允许我们处理用户输入

```rust
io::stdin().read_line(&mut guess)
```

也可以通过 `std::io::stdin` 来使用该函数。`stdin` 函数返回 `std::io::Stdin` 的实例，它是表示终端标准输入句子的类型。

`.read_line(&mut guess)` 调用标准输入句柄上的 `read_line` 方法以获取用户的输入。同时将 `&mut guess` 作为参数传递给 `read_line` ，告诉它存储用户输入的字符串。 `read_line` 的完整工作是获取任何用户输入的内容并将其附加到字符串中（不覆盖其内容），因此将该字符串作为参数传递。

`&` 表示该参数是一个引用，引用可以达到代码的多个部分访问一条数据的方法，而无需多次将该数据复制到内存中。

`Rust` 的主要优点之一是使用引用的安全性和易用性。

与变量一样，默认情况下引用是不可变的，因此需要编写 `&mut guess` 而不是 `&guess` 来使其可变。

#### `read_line` 将用户输入的任何内容放入我们传递给它的字符串中，但它也返回一个 `Result` 值。 `Result` 是一个枚举，通常称为枚举，它是一种可以处于多种可能状态之一的类型。将每种可能的状态称为变体。

这些 `Result` 类型的目的是对错误处理信息进行编码。

`Result` 类型的值与任何类型的值一样，都定义了方法。 `Result` 的实例有一个可以调用的 `expect` 方法。如果 `Result` 的此实例是 `Err` 值， `expect` 将导致程序崩溃并显示您作为参数传递给 `expect` 方法返回 `Err` ，则它可能是来自底层操作系统的错误的结果。如果 `Result` 的此实例是 `Ok` 值，则 `expect` 将采用 `Ok` 所保存的返回值并将该值返回给你这样你就可以使用它。在本例中，该值是用户输入中的字节数。

```rust
io::stdin()
        .read_line(&mut guess)
        .expect("Failed to read line"); // .expect("Failed to read line");
```

#### `{}` 组大括号是一个占位符：将 `{}` 想象成固定值的小螃蟹钳子。打印变量值时，变量名称可以放在大括号内。打印表达式求值结果时，将空大括号放在格式字符串中，然后在格式字符串后跟上以逗号分隔的表达式列表，以相同的顺序在每个空大括号占位符中打印。在对 `println!` 的一次调用中打印变量和表达式的结果如下所示

```rust
let x = 5;
let y = 10;

println!("x = {x} and y + 2 = {}", y + 2);
// x = 5 and y + 2 = 12
```

#### 使用 `crate` 

Rust中的包（crate）代表了一系列源代码文件的集合。我们当前正在构建的项目是一个用于生成可执行程序的二进制包（binary crate），而我们引用的rand包则是一个用于复用功能的库包（library crate，代码包）。

需要修改 `Cargo.toml` 文件以包含 `rand` 包作为依赖项，在 `[dependencies]` 部门标题下方添加包的名字和版本号

```rust
[dependencies]
rand = "0.8.5"
```

在 Cargo.toml 文件中，标头后面的所有内容都是该部分的一部分，一直持续到另一个部分开始。在 `[dependencies]` 中，您告诉 Cargo 您的项目依赖哪些外部 crate，以及您需要这些 crate 的哪些版本。在本例中，我们使用语义版本说明符 `0.8.5` 指定 `rand` 箱。 Cargo 了解语义版本控制（有时称为 SemVer），这是编写版本号的标准。说明符 `0.8.5` 实际上是 `^0.8.5` 的简写，这意味着至少 0.8.5 但低于 0.9.0 的任何版本。

Cargo 认为这些版本具有与 0.8.5 版本兼容的公共 API，并且此规范确保您将获得仍将使用本章中的代码进行编译的最新补丁版本。不保证任何 0.9.0 或更高版本具有与以下示例使用的 API 相同的 API。

然后重新构建项目。构建后显示的版本号可能会与我们指定的不同，但多亏了 `SemVer` 的约定，它们会与我们的代码保持兼容。

现在程序有了一个外部依赖，Cargo可以从注册表（registry）中获取所有可用库的最新版本信息，而这些信息通常是从 [crates.io](https://crates.io/) 上复制过来的。在Rust的生态中，crates.io是人们用于分享各种各样开源Rust项目的网站。

更新注册表后，Cargo 检查 `[dependencies]` 部分并下载列出的所有尚未下载的 crate。在本例中，虽然我们只将 `rand` 列为依赖项，但 Cargo 还获取了 `rand` 依赖的其他 crate 来工作。下载包后，Rust 会编译它们，然后使用可用的依赖项编译项目。

你可能会注意到，虽然我们只将rand引用为依赖，但Cargo却额外下载了一份libc的数据，这是因为rand本身是依赖于libc来完成工作的。

#### Cargo.lock 文件

Cargo 具有一种机制，可确保您或其他任何人每次构建代码时都可以重建相同的工件：Cargo 将仅使用您指定的依赖项版本，直到您另有指示。例如，假设下周 `rand` 包的 0.8.6 版本将发布，该版本包含一个重要的错误修复，但它也包含一个会破坏您的代码的回归。为了处理这个问题，Rust 在您第一次运行 `cargo build` 时创建了 Cargo.lock 文件，所以我们现在将其放置在guessing_game 目录中。

当您第一次构建项目时，Cargo 会找出符合条件的所有依赖项版本，然后将它们写入 Cargo.lock 文件。当您将来构建项目时，Cargo 将看到 Cargo.lock 文件存在，并将使用其中指定的版本，而不是再次执行确定版本的所有工作。这可以让您自动获得可重复的构建。换句话说，在您明确升级之前，您的项目将保持在 0.8.5，这要归功于 Cargo.lock 文件。由于 Cargo.lock 文件对于可重现的构建非常重要，因此它通常会与项目中的其余代码一起签入源代码管理。

#### 使用 `cargo update` 更新 `crate` 

当您确实想要更新 crate 时，Cargo 会提供命令 `update` ，该命令将忽略 Cargo.lock 文件并找出符合您在 Cargo.toml 中的规范的所有最新版本。然后 Cargo 会将这些版本写入 Cargo.lock 文件。否则，默认情况下，Cargo 将仅查找大于 0.8.5 且小于 0.9.0 的版本。如果 `rand` 箱已发布两个新版本 0.8.6 和 0.9.0，则运行 `cargo update` 时您将看到以下内容：

```shell
cargo update
    Updating crates.io index
    Updating rand v0.8.5 -> v0.8.6
```

Cargo 忽略 0.9.0 版本。此时，您还会注意到 Cargo.lock 文件中的更改，指出您现在使用的 `rand` 箱的版本是 0.8.6。要使用 `rand` 版本 0.9.0 或 0.9.x 系列中的任何版本，您必须将 Cargo.toml 文件更新为如下所示：

```toml
[dependencies]
rand = "0.9.0"
```

下次运行 `cargo build` 时，Cargo 将更新可用 crate 的注册表，并根据您指定的新版本重新评估您的 `rand` 要求。

#### 生成随机数

```rust
use rand::Rng;
...
let secret_number = rand::thread_rng().gen_range(1..=100);
```

额外增加了一行use语句：use rand::Rng。这里的Rng是一个trait（特征），它定义了随机数生成器需要实现的方法集合。为了使用这些方法，我们需要显式地将它引入当前的作用域中。

在第一行中，我们调用 `rand::thread_rng` 函数，它为我们提供了我们将要使用的特定随机数生成器：一个位于当前执行线程本地并由操作系统播种的随机数生成器。然后我们调用随机数生成器上的 `gen_range` 方法。此方法由我们通过 `use rand::Rng;` 语句引入范围的 `Rng` 特征定义。 `gen_range` 方法采用范围表达式作为参数，并在该范围内生成一个随机数。我们在这里使用的范围表达式类型采用 `start..=end` 形式，并且包含下限和上限，因此我们需要指定 `1..=100` 来请求 1 到 100 之间的数字。

>   如果想知道使用哪些特征以及要从包中调用哪些方法和函数，因此每个包都有包含使用说明的文档。 Cargo 的另一个巧妙功能是运行 `cargo doc --open` 命令将在本地构建所有依赖项提供的文档并在浏览器中打开它。例如，如果您对 `rand` 包中的其他功能感兴趣，请运行 `cargo doc --open` 并单击左侧边栏中的 `rand` 。

#### 比较

我们添加另一个 `use` 语句，将名为 `std::cmp::Ordering` 的类型从标准库引入作用域。 `Ordering` 类型是另一个枚举，具有变体 `Less` 、 `Greater` 和 `Equal` 。这是比较两个值时可能出现的三种结果。

```rust
match guess.cmp(&secret_number){
    Ordering::Less => println!("Too Small!"),
    Ordering:: Greater => println!("Too Big!"),
    Ordering::Equal => println!("You Win!")
}
```

Rust 拥有强大的静态类型系统。然而，它也有类型推断。当我们编写 `let mut guess = String::new()` 时，Rust 能够推断出 `guess` 应该是 `String` 并且没有让我们编写类型。另一方面， `secret_number` 是数字类型。 Rust 的一些数字类型的值可以在 1 到 100 之间： `i32` ，一个 32 位数字； `u32` ，无符号 32 位数字； `i64` ，一个 64 位数字；以及其他人。除非另有指定，Rust 默认为 `i32` ，它是 `secret_number` 的类型，除非您在其他地方添加类型信息，这会导致 Rust 推断出不同的数字类型。错误的原因是 Rust 无法比较字符串和数字类型。

Rust 允许我们用新值来隐藏 `guess` 的先前值，这很有帮助。阴影允许我们重用 `guess` 变量名称，而不是强迫我们创建两个唯一的变量，例如 `guess_str` 和 `guess` 。当想要将值从一种类型转换为另一种类型时，通常会使用此功能。

`String` 实例上的 `trim` 方法将消除开头和结尾的任何空格。`trim` 方法消除 `\n` 或 `\r\n` ，结果只是 `5` 。

字符串上的 `parse` 方法将字符串转换为另一种类型。在这里，我们使用它从字符串转换为数字。我们需要使用 `let guess: u32` 告诉 Rust 我们想要的确切数字类型。

此示例程序中的 `u32` 注释以及与 `secret_number` 的比较意味着 Rust 会推断 `secret_number` 也应该是 `u32` 。

`parse` 方法仅适用于逻辑上可以转换为数字的字符，因此很容易导致错误。例如，如果字符串包含 `A👍%` ，则无法将其转换为数字。因为它可能会失败，所以 `parse` 方法返回 `Result` 类型。

```rust
let guess: u32 = guess.trim().parse().expect("Please type a number!");
...
match guess.cmp(&secret_number){
    Ordering::Less => println!("Too Small!"),
    Ordering:: Greater => println!("Too Big!"),
    Ordering::Equal => println!("You Win!")
}
```

#### loop

`loop` 关键字创建无限循环。

在 `You win!` 之后添加 `break` 行会使程序在用户正确猜测秘密数字时退出循环。退出循环也意味着退出程序，因为循环是 `main` 的最后一部分。

```rust
loop {
    println!("Please input your guess.");

    match guess.cmp(&secret_number) {
        Ordering::Less => println!("Too Small!"),
        Ordering::Greater => println!("Too Big!"),
        Ordering::Equal => {
            println!("You Win!");
            break;
        },
    }
}
```

#### 处理无效输入

为了进一步完善游戏的行为，让游戏忽略非数字以便用户可以继续猜测，而不是在用户输入非数字时使程序崩溃。我们可以通过更改将 `guess` 从 `String` 转换为 `u32` 的行来实现这一点。

```rust
let guess: u32 = match guess.trim().parse(){
    Ok(num) => num,
    Err(_) => continue,
};
```

我们从 `expect` 调用切换到 `match` 表达式，以从因错误而崩溃转向处理错误。请记住， `parse` 返回 `Result` 类型，而 `Result` 是一个具有变体 `Ok` 和 `Err` 的枚举。我们在这里使用 `match` 表达式，就像我们对 `cmp` 方法的 `Ordering` 结果所做的那样。

如果 `parse` 能够成功将字符串转换为数字，它将返回包含结果数字的 `Ok` 值。该 `Ok` 值将匹配第一个手臂的模式，并且 `match` 表达式将仅返回 `parse` 生成并放入其中的 `num` 值 `Ok` 值。该数字最终将位于我们正在创建的新 `guess` 变量中我们想要的位置。

如果 `parse` 无法将字符串转换为数字，它将返回一个 `Err` 值，其中包含有关错误的更多信息。 `Err` 值与第一个 `match` 臂中的 `Ok(num)` 模式不匹配，但与第二个臂中的 `Err(_)` 模式匹配。下划线 `_` 是一个包罗万象的值；在这个例子中，我们说我们想要匹配所有 `Err` 值，无论它们里面有什么信息。因此，程序将执行第二个分支的代码 `continue` ，它告诉程序转到 `loop` 的下一次迭代并要求进行另一个猜测。因此，实际上，程序会忽略 `parse` 可能遇到的所有错误！

# 完整代码

```rust
use rand::Rng;
use std::cmp::Ordering;
use std::io;

fn main() {
    println!("Guessing the number!");

    let secret_number = rand::thread_rng().gen_range(1..=100);

    loop {
        println!("Please input your guess.");

        let mut guess = String::new();

        io::stdin()
            .read_line(&mut guess)
            .expect("Failed to read line");

        let guess: u32 = match guess.trim().parse(){
            Ok(num) => num,
            Err(_) => {
                println!("Please type a number!");
                continue;
            },
        };

        println!("Your guessed: {guess}");

        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too Small!"),
            Ordering::Greater => println!("Too Big!"),
            Ordering::Equal => {
                println!("You Win!");
                break;
            },
        }
    }
}
```

