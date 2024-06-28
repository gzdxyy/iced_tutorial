
# 容器

[Container](https://docs.rs/iced/0.12.1/iced/widget/container/struct.Container.html) 是另一种帮助我们布局控件的方法，特别是当我们需要在水平和垂直方向上都居中对齐控件时。
[Container](https://docs.rs/iced/0.12.1/iced/widget/container/struct.Container.html) 可以接收任何控件，包括 [Column](https://docs.rs/iced/0.12.1/iced/widget/struct.Column.html) 和 [Row](https://docs.rs/iced/0.12.1/iced/widget/struct.Row.html)。

```rust
use iced::{
    alignment::{Horizontal, Vertical},
    widget::{column, container, Container},
    Length, Sandbox, Settings,
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
            Container::new("通过结构体构造"),
            container("通过函数构造"),
            container("带填充").padding(20),
            container("不同的水平对齐")
                .width(Length::Fill)
                .align_x(Horizontal::Center),
            container("不同的垂直对齐轴")
                .height(Length::Fill)
                .align_y(Vertical::Center),
        ]
        .into()
    }
}
```

![容器](./pic/container.png)

如果我们想要在水平和垂直方向上都居中一个控件，我们可以使用以下代码：

```rust
container("中心").width(Length::Fill).height(Length::Fill).center_x().center_y()
```

:arrow_right: 下一步：[可滚动](./scrollable.md)

:blue_book: 返回：[目录](./../README.md)
