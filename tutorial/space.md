
# Space

[Space](https://docs.rs/iced/0.12.1/iced/widget/space/struct.Space.html) 是一个方便的控件，帮助我们进行控件布局。
它是一个占据空间的空控件。
它有几种构造方法，帮助我们在水平、垂直或两者方向上分配空间。

```rust
use iced::{
    widget::{button, column, horizontal_space, row, vertical_space, Space},
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
            row![
                button("水平空白 1A"),
                Space::with_width(50),
                button("水平空白 1B"),
            ],
            row![
                button("水平空白 2A"),
                Space::with_width(Length::Fill),
                button("水平空白 2B"),
            ],
            row![
                button("水平空白 3A"),
                horizontal_space(),
                button("水平空白 3B"),
            ],
            button("垂直空白 1A"),
            Space::with_height(50),
            button("垂直空白 1B"),
            Space::with_height(Length::Fill),
            button("垂直空白 2A"),
            vertical_space(),
            button("垂直空白 2B"),
            button("对角线空白 A"),
            row![Space::new(50, 50), button("对角线空白 B")].align_items(Alignment::End)
        ]
        .into()
    }
}
```

![空白](./pic/space.png)

:arrow_right: 下一步：[容器](./container.md)

:blue_book: 返回：[目录](./../README.md)
