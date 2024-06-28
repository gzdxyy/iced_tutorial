
# 通过计时器生成消息

要使用内置的计时器，我们需要启用以下特性之一：[tokio](https://docs.rs/crate/iced/0.12.1/features#tokio)、[async-std](https://docs.rs/crate/iced/0.12.1/features#async-std) 或 [smol](https://docs.rs/crate/iced/0.12.1/features#smol)。
在本教程中，我们使用 [tokio](https://docs.rs/crate/iced/0.12.1/features#tokio) 特性。
`Cargo.toml` 的依赖应该如下所示：

```toml
[dependencies]
iced = { version = "0.12.1", features = ["tokio"] }
```

我们使用 [time::every](https://docs.rs/iced/0.12.1/iced/time/fn.every.html) 函数来获取 [Subscription](https://docs.rs/iced/0.12.1/iced/struct.Subscription.html)<[Instant](https://docs.rs/iced/0.12.1/iced/time/struct.Instant.html)> 结构体。
然后我们通过 [Subscription::map](https://docs.rs/iced/0.12.1/iced/struct.Subscription.html#method.map) 方法将该结构体映射为 [Subscription](https://docs.rs/iced/0.12.1/iced/struct.Subscription.html)<`MyAppMessage`>。
结果将在 [Application](https://docs.rs/iced/0.12.1/iced/application/trait.Application.html) 的 [subscription](https://docs.rs/iced/0.12.1/iced/application/trait.Application.html#method.subscription) 方法中返回。
相应的 `MyAppMessage` 将在 [update](https://docs.rs/iced/0.12.1/iced/application/trait.Application.html#tymethod.update) 方法中接收。

```rust
use iced::{
    executor,
    time::{self, Duration},
    widget::text,
    Application, Command, Settings,
};

fn main() -> iced::Result {
    MyApp::run(Settings::default())
}

#[derive(Debug, Clone)]
enum MyAppMessage {
    Update,
}

struct MyApp {
    seconds: u32,
}

impl Application for MyApp {
    type Executor = executor::Default;
    type Message = MyAppMessage;
    type Theme = iced::Theme;
    type Flags = ();

    fn new(_flags: Self::Flags) -> (Self, iced::Command<Self::Message>) {
        (Self { seconds: 0 }, Command::none())
    }

    fn title(&self) -> String {
        String::from("My App")
    }

    fn update(&mut self, message: Self::Message) -> iced::Command<Self::Message> {
        match message {
            MyAppMessage::Update => self.seconds += 1,
        }
        Command::none()
    }

    fn view(&self) -> iced::Element<Self::Message> {
        text(self.seconds.to_string()).into()
    }

    fn subscription(&self) -> iced::Subscription<Self::Message> {
        time::every(Duration::from_secs(1)).map(|_| MyAppMessage::Update)
    }
}
```

![通过计时器生成消息](./pic/producing_messages_by_timers.png)

:arrow_right: 下一步：[批量订阅](./batch_subscriptions.md)

:blue_book: 返回：[目录](./../README.md)
