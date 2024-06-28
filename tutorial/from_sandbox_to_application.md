
# 从 Sandbox 到 Application

为了在我们的应用程序中拥有更多的控制权，我们可以使用 [Application](https://docs.rs/iced/0.12.1/iced/application/trait.Application.html) trait，它是 [Sandbox](https://docs.rs/iced/0.12.1/iced/trait.Sandbox.html) trait 的一个泛化。

[Application](https://docs.rs/iced/0.12.1/iced/application/trait.Application.html) 和 [Sandbox](https://docs.rs/iced/0.12.1/iced/trait.Sandbox.html) 之间有两个主要区别。

一是 [关联类型](https://doc.rust-lang.org/stable/book/ch19-03-advanced-traits.html#specifying-placeholder-types-in-trait-definitions-with-associated-types)。在 [Sandbox](https://docs.rs/iced/0.12.1/iced/trait.Sandbox.html) 中，除了 [Message](https://docs.rs/iced/0.12.1/iced/trait.Sandbox.html#associatedtype.Message) 外，我们还必须指定 [Executor](https://docs.rs/iced/0.12.1/iced/application/trait.Application.html#associatedtype.Executor)、[Theme](https://docs.rs/iced/0.12.1/iced/application/trait.Application.html#associatedtype.Theme) 和 [Flags](https://docs.rs/iced/0.12.1/iced/application/trait.Application.html#associatedtype.Flags)。基本上，我们使用这些关联类型的建议默认值。

二是我们必须在 [new](https://docs.rs/iced/0.12.1/iced/application/trait.Application.html#tymethod.new) 方法和 [update](https://docs.rs/iced/0.12.1/iced/application/trait.Application.html#tymethod.update) 方法中返回 [Command](https://docs.rs/iced/0.12.1/iced/struct.Command.html)。我们两个方法都返回 [Command::none()](https://docs.rs/iced/0.12.1/iced/struct.Command.html#method.none)。

```rust
use iced::{executor, Application, Command, Settings};

fn main() -> iced::Result {
    MyApp::run(Settings::default())
}

struct MyApp;

impl Application for MyApp {
    type Executor = executor::Default;
    type Message = ();
    type Theme = iced::Theme;
    type Flags = ();

    fn new(_flags: Self::Flags) -> (Self, iced::Command<Self::Message>) {
        (Self, Command::none())
    }

    fn title(&self) -> String {
        String::from("My App")
    }

    fn update(&mut self, _message: Self::Message) -> iced::Command<Self::Message> {
        Command::none()
    }

    fn view(&self) -> iced::Element<Self::Message> {
        "Hello World!".into()
}
```

![从 Sandbox 到 Application](./pic/from_sandbox_to_application.png)

:arrow_right: 下一步：[通过命令控制控件](./controlling_widgets_by_commands.md)

:blue_book: 返回：[目录](./../README.md)
