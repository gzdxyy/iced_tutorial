
# 初始化不同的窗口

我们可以使用 [window::Settings](https://docs.rs/iced/0.12.1/iced/window/settings/struct.Settings.html) 在调用 [Sandbox](https://docs.rs/iced/0.12.1/iced/trait.Sandbox.html) 或 [Application](https://docs.rs/iced/0.12.1/iced/application/trait.Application.html) 的 [run](https://docs.rs/iced/0.12.1/iced/trait.Sandbox.html#method.run) 时更改窗口的属性（例如 [position](https://docs.rs/iced/0.12.1/iced/window/settings/struct.Settings.html#structfield.position) 和 [size](https://docs.rs/iced/0.12.1/iced/window/settings/struct.Settings.html#structfield.size)）。
开发人员可能对阅读 [window::Settings](https://docs.rs/iced/0.12.1/iced/window/settings/struct.Settings.html) 文档中的其他属性感兴趣。

```rust
use iced::{window, Point, Sandbox, Settings, Size};

fn main() -> iced::Result {
    MyApp::run(Settings {
        window: window::Settings {
            size: Size {
                width: 70.,
                height: 30.,
            },
            position: window::Position::Specific(Point { x: 50., y: 60. }),
            ..window::Settings::default()
        },
        ..Settings::default()
    })
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
}
```

![初始化不同的窗口](./pic/initializing_a_different_window.png)

:arrow_right: 下一步：[动态更改窗口](./changing_the_window_dynamically.md)

:blue_book: 返回：[目录](./../README.md)
