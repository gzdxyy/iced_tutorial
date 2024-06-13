# 第一个应用 - Hello World!

我们需要一个结构体来实现 [Sandbox](https://docs.rs/iced/0.12.1/iced/trait.Sandbox.html) 并从 `main` 函数中调用它的 [run](https://docs.rs/iced/0.12.1/iced/trait.Sandbox.html#method.run) 方法。
所有的控件应该放置在 [view](https://docs.rs/iced/0.12.1/iced/trait.Sandbox.html#tymethod.view) 方法内。

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
        "Hello World!".into()
    }
}
```

![第一个应用](./pic/first_app.png)

:arrow_right: 下一步：[Sandbox Trait 说明](./explanation_of_sandbox_trait.md)

:blue_book: 返回：[目录](./../README.md)
