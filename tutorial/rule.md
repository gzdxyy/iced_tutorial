# 分隔线

[Rule](https://docs.rs/iced/0.12.1/iced/widget/rule/struct.Rule.html) 组件是用于清晰分隔控件的水平（或垂直）线。
它有两种构造方法。
我们可以改变它周围的空间。
该组件可以设置为水平或垂直。

```rust
use iced::{
    widget::{column, horizontal_rule, text, vertical_rule, Rule},
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
            text("通过结构体构造"),
            Rule::horizontal(0),
            text("通过函数构造"),
            horizontal_rule(0),
            text("不同的空间"),
            horizontal_rule(50),
            text("垂直分隔线"),
            vertical_rule(100),
        ]
        .into()
    }
}
```

![分隔线](./pic/rule.png)

:arrow_right: 下一步：[图像](./image.md)

:blue_book: 返回：[目录](./../README.md)
