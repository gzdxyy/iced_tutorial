
# 更改主题

我们可以在 [Sandbox](https://docs.rs/iced/0.12.1/iced/trait.Sandbox.html) 中实现 [theme](https://docs.rs/iced/0.12.1/iced/trait.Sandbox.html#method.theme) 方法来返回所需的主题。

```rust
use iced::{Sandbox, Settings};

fn main() -> iced::Result {
    MyApp::run(Settings::default())
}

struct MyApp;

impl Sandbox for MyApp {
    type Message = ();

    fn new() -> Self {
        Self
    }

    fn title(&self) -> String {
        String::from("My App")
    }

    fn update(&mut self, _message: Self::Message) {}

    fn view(&self) -> iced::Element<Self::Message> {
        "Hello".into()
    }

    fn theme(&self) -> iced::Theme {
        iced::Theme::Dark
        // 或者
        // iced::Theme::Light
    }
}
```

![更改主题](./pic/changing_themes.png)

:arrow_right: 下一步：[更改样式](./changing_styles.md)

:blue_book: 返回：[目录](./../README.md)
