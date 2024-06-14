# 更改显示内容

要动态更改 [view](https://docs.rs/iced/0.12.1/iced/trait.Sandbox.html#tymethod.view) 中的内容，我们可以执行以下操作：

* 在主结构体 `MyApp` 中添加一些字段（例如 `counter`）。
* 在 [view](https://docs.rs/iced/0.12.1/iced/trait.Sandbox.html#tymethod.view) 中显示这些字段（例如 `text(self.counter)`）。
* 在 [view](https://docs.rs/iced/0.12.1/iced/trait.Sandbox.html#tymethod.view) 中产生一些消息（例如 `button(...).on_press(MyAppMessage::ButtonPressed)`）。
* 当接收到消息时，在 [update](https://docs.rs/iced/0.12.1/iced/trait.Sandbox.html#tymethod.update) 中更新字段（例如 `self.counter += 1`）。

```rust
use iced::{
    widget::{button, column, text},
    Sandbox, Settings,
};

fn main() -> iced::Result {
    MyApp::run(Settings::default())
}

#[derive(Debug, Clone)]
enum MyAppMessage {
    ButtonPressed,
}

struct MyApp {
    counter: usize,
}

impl Sandbox for MyApp {
    type Message = MyAppMessage;

    fn new() -> Self {
        Self { counter: 0 }
    }

    fn title(&self) -> String {
        String::from("My App")
    }

    fn update(&mut self, message: Self::Message) {
        match message {
            MyAppMessage::ButtonPressed => self.counter += 1,
        }
    }

    fn view(&self) -> iced::Element<Self::Message> {
        column![
            text(self.counter.to_string()),
            button("Increase").on_press(MyAppMessage::ButtonPressed),
        ]
        .into()
    }
}
```

![产生和接收消息](./pic/changing_displaying_content.png)

:arrow_right: 下一步：[文本](./text.md)

:blue_book: 返回：[目录](./../README.md)
