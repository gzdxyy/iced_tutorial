
# Row

类似于 [Column](https://docs.rs/iced/0.12.1/iced/widget/struct.Column.html)，[Row](https://docs.rs/iced/0.12.1/iced/widget/struct.Row.html) 帮助我们水平地放置控件。
它可以在其边界和其内部内容之间留出一些空间。
它还可以在其内部控件之间添加空间。
内部控件可以顶部对齐、居中对齐或底部对齐。

```rust
use iced::{
    widget::{column, row, Row},
    Alignment, Length, Sandbox, Settings,
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
            Row::with_children(vec![
                "通过 with_children 函数构造。".into(),
                "另一个元素".into(),
            ]),
            Row::new()
                .push("通过 new 函数和 push 方法构造。")
                .push("另一个元素"),
            row(vec!["通过函数构造".into()]),
            row!["通过宏构造"],
            row!["带填充"].padding(20),
            row!["元素之间的空间", "元素之间的空间"].spacing(20),
            row!["不同的对齐方式"]
                .height(Length::Fill)
                .align_items(Alignment::Center),
        ]
        .into()
    }
}
```

![行](./pic/row.png)

:arrow_right: 下一步：[空白](./space.md)

:blue_book: 返回：[目录](./../README.md)
