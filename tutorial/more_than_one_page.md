
# 超过一个页面

要拥有多个页面，我们可以在主结构体 `MyApp` 中添加一个 `page` 字段。
字段 `page` 是一个由我们定义的 [enum](https://doc.rust-lang.org/std/keyword.enum.html)，它决定了在 [Sandbox](https://docs.rs/iced/0.12.1/iced/trait.Sandbox.html) 的 [view](https://docs.rs/iced/0.12.1/iced/trait.Sandbox.html#tymethod.view) 方法中显示什么。

```rust
use iced::{
    widget::{button, column, text},
    Sandbox, Settings,
};

fn main() -> iced::Result {
    MyApp::run(Settings::default())
}

enum Page {
    A,
    B,
}

#[derive(Debug, Clone)]
enum MyAppMessage {
    GoToBButtonPressed,
    GoToAButtonPressed,
}

struct MyApp {
    page: Page,
}

impl Sandbox for MyApp {
    type Message = MyAppMessage;

    fn new() -> Self {
        Self { page: Page::A }
    }

    fn title(&self) -> String {
        String::from("My App")
    }

    fn update(&mut self, message: Self::Message) {
        self.page = match message {
            MyAppMessage::GoToBButtonPressed => Page::B,
            MyAppMessage::GoToAButtonPressed => Page::A,
        };
    }

    fn view(&self) -> iced::Element<Self::Message> {
        match self.page {
            Page::A => {
                column![
                    text("Page A"),
                    button("Go to B")
                        .on_press(MyAppMessage::GoToBButtonPressed),
                ]
            }
            Page::B => {
                column![
                    text("Page B"),
                    button("Go to A")
                        .on_press(MyAppMessage::GoToAButtonPressed),
                ]
            }
        }
        .into()
    }
}
```

页面 A：

![页面 A](./pic/more_than_one_page_a.png)

页面 B：

![页面 B](./pic/more_than_one_page_b.png)

:arrow_right: 下一步：[无记忆页面](./memoryless_pages.md)

:blue_book: 返回：[目录](./../README.md)
