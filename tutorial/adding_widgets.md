# 添加控件

使用 [column!](https://docs.rs/iced/0.12.1/iced/widget/macro.column.html) 和 [row!](https://docs.rs/iced/0.12.1/iced/widget/macro.row.html) 宏来组合多个控件，比如 [text](https://docs.rs/iced/0.12.1/iced/widget/fn.text.html) 和 [button](https://docs.rs/iced/0.12.1/iced/widget/fn.button.html)。

```rust
use iced::{
    widget::{button, column, row, text},
    Sandbox, Settings,
};

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
        column![
            text("Yes or no?"),
            row![
                button("Yes"),
                button("No"),
            ],
        ].into()
    }
}
```

![添加控件](./pic/adding_widgets.png)

:arrow_right: 下一步：[更改显示内容](./changing_displaying_content.md)

:blue_book: 返回：[目录](./../README.md)
