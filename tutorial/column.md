# 列（Column）

[Column](https://docs.rs/iced/0.12.1/iced/widget/struct.Column.html) 帮助我们垂直地放置控件。
它可以在其边界和其内部内容之间留出一些空间。
它还可以在其内部控件之间添加空间。
内部控件可以左对齐、居中对齐或右对齐。

```rust
use iced::{
    widget::{column, Column},
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
            Column::with_children(vec![
                "通过 with_children 函数构造".into(),
                "另一个元素".into(),
            ]),
            Column::new()
                .push("通过 new 函数和 push 方法构造")
                .push("另一个元素"),
            column(vec!["通过函数构造".into()]),
            column!["通过宏构造"],
            column!["带填充"].padding(20),
            column!["不同的对齐方式"]
                .width(Length::Fill)
                .align_items(Alignment::Center),
            column!["元素之间的空间", "元素之间的空间"]
                .spacing(20),
        ]
        .into()
    }
}
```

![列](./pic/column.png)

:arrow_right: 下一步：[行（Row）](./row.md)

:blue_book: 返回：[目录](./../README.md)
