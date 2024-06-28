
# 执行自定义命令

[Commands](https://docs.rs/iced/0.12.1/iced/struct.Command.html) 可以帮助我们执行异步函数。
为此，我们需要启用以下特性之一：[tokio](https://docs.rs/crate/iced/0.12.1/features#tokio)、[async-std](https://docs.rs/crate/iced/0.12.1/features#async-std) 或 [smol](https://docs.rs/crate/iced/0.12.1/features#smol)。
还必须添加相应的异步运行时（[tokio](https://crates.io/crates/tokio)、[async-std](https://crates.io/crates/async-std) 或 [smol](https://crates.io/crates/smol)）。

这里，我们以 [tokio](https://crates.io/crates/tokio) 为例。
我们启用 [tokio](https://docs.rs/crate/iced/0.12.1/features#tokio) 特性并添加 [tokio](https://crates.io/crates/tokio) crate。
`Cargo.toml` 的依赖项应该如下所示：

```toml
[dependencies]
iced = { version = "0.12.1", features = ["tokio"] }
tokio = { version = "1.37.0", features = ["time"] }
```

我们使用 [Command::perform](https://docs.rs/iced/0.12.1/iced/struct.Command.html#method.perform) 来执行异步函数。
[Command::perform](https://docs.rs/iced/0.12.1/iced/struct.Command.html#method.perform) 的第一个参数是一个异步函数，第二个参数是一个返回 `MyAppMessage` 的函数。
一旦异步函数完成，我们将收到 `MyAppMessage::Done`。

在以下代码中，我们使用了一个简单的异步函数 [tokio::time::sleep](https://docs.rs/tokio/latest/tokio/time/fn.sleep.html)。
当异步函数完成时，我们将收到 `MyAppMessage::Done`。

```rust
use std::time::Duration;

use iced::{
    executor,
    widget::{button, column, text},
    Application, Command, Settings,
};

fn main() -> iced::Result {
    MyApp::run(Settings::default())
}

#[derive(Debug, Clone)]
enum MyAppMessage {
    Execute,
    Done,
}

struct MyApp {
    state: String,
}

impl Application for MyApp {
    type Executor = executor::Default;
    type Message = MyAppMessage;
    type Theme = iced::Theme;
    type Flags = ();

    fn new(_flags: Self::Flags) -> (Self, iced::Command<Self::Message>) {
        (
            Self {
                state: "Ready".into(),
            },
            Command::none(),
        )
    }

    fn title(&self) -> String {
        String::from("My App")
    }

    fn update(&mut self, message: Self::Message) -> iced::Command<Self::Message> {
        match message {
            MyAppMessage::Execute => {
                self.state = "Executing".into();
                return Command::perform(tokio::time::sleep(Duration::from_secs(1)), |_| {
                    MyAppMessage::Done
                });
            }
            MyAppMessage::Done => self.state = "Done".into(),
        }
        Command::none()
    }

    fn view(&self) -> iced::Element<Self::Message> {
        column![
            button("Execute").on_press(MyAppMessage::Execute),
            text(self.state.as_str()),
        ]
        .into()
    }
}
```

![执行自定义命令](./pic/executing_custom_commands.png)

:arrow_right: 下一步：[初始化不同的窗口](./initializing_a_different_window.md)

:blue_book: 返回：[目录](./../README.md)
