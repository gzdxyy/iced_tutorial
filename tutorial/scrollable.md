
# Scrollable

当控件数量过多时，它们可能会超出窗口的边界。
[Scrollable](https://docs.rs/iced/0.12.1/iced/widget/scrollable/struct.Scrollable.html) 提供了一个无限空间，控件可以通过滚动条进行导航。
滚动条可以是垂直的、水平的或两者兼有。
当滚动条发生变化时，我们还可以接收它们滚动位置并更新其他控件。

```rust
use iced::{
    widget::{
        column, row,
        scrollable::{Direction, Properties, Viewport},
        text, Scrollable,
    },
    Sandbox, Settings,
};

fn main() -> iced::Result {
    MyApp::run(Settings::default())
}

#[derive(Debug, Clone)]
enum MyMessage {
    Scrolled4(Viewport),
}

struct MyApp {
    offset4: String,
}

impl Sandbox for MyApp {
    type Message = MyMessage;

    fn new() -> Self {
        Self { offset4: "".into() }
    }

    fn title(&self) -> String {
        String::from("My App")
    }

    fn update(&mut self, message: Self::Message) {
        match message {
            MyMessage::Scrolled4(v) => {
                self.offset4 = format!("{} {}", v.absolute_offset().x, v.absolute_offset().y)
            }
        }
    }

    fn view(&self) -> iced::Element<Self::Message> {
        let long_vertical_texts =
            column((0..10).map(|i| text(format!("{} 垂直滚动", i + 1)).into()));
        let long_horizontal_texts =
            row((0..10).map(|i| text(format!("{} 水平滚动", i + 1)).into()));
        let long_both_texts = column(
            (0..10).map(|i| text(format!("{} 垂直和水平滚动", i + 1)).into()),
        );
        let long_both_texts_2 = column(
            (0..10).map(|i| text(format!("{} 垂直和水平滚动", i + 1)).into()),
        );

        column![
            Scrollable::new(long_vertical_texts)
                .width(230)
                .height(105)
                .direction(Direction::Vertical(Properties::new())),
            Scrollable::new(long_horizontal_texts)
                .width(500)
                .height(30)
                .direction(Direction::Horizontal(Properties::new())),
            Scrollable::new(long_both_texts)
                .width(230)
                .height(105)
                .direction(Direction::Both {
                    vertical: Properties::new(),
                    horizontal: Properties::new()
                }),
            column![
                Scrollable::new(long_both_texts_2)
                    .width(230)
                    .height(105)
                    .direction(Direction::Both {
                        vertical: Properties::new(),
                        horizontal: Properties::new()
                    })
                    .on_scroll(MyMessage::Scrolled4),
                text(&self.offset4),
            ],
        ]
        .spacing(50)
        .into()
    }
}
```

![可滚动](./pic/scrollable.png)

除了使用 [Scrollable::new](https://docs.rs/iced/0.12.1/iced/widget/scrollable/struct.Scrollable.html#method.new)，我们还可以使用 [scrollable](https://docs.rs/iced/0.12.1/iced/widget/fn.scrollable.html) 函数。

:arrow_right: 下一步：[更改主题](./changing_themes.md)

:blue_book: 返回：[目录](./../README.md)
