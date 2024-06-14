# Toggler

[Toggler](https://docs.rs/iced/0.12.1/iced/widget/toggler/struct.Toggler.html) 组件代表一个布尔值。
它有两种构造方法。
它支持对点击和触摸的反应。
它可以改变按钮和文本的样式以及它们之间的间距。
它还可以对齐其文本。

```rust
use iced::{
    alignment::Horizontal,
    font::Family,
    widget::{column, text::Shaping, toggler, Toggler},
    Font, Sandbox, Settings,
};

fn main() -> iced::Result {
    MyApp::run(Settings::default())
}

#[derive(Debug, Clone)]
enum MyAppMessage {
    DoNothing,
    Update3(bool),
    Update4(bool),
}

#[derive(Default)]
struct MyApp {
    toggler3: bool,
    toggler4: bool,
}

impl Sandbox for MyApp {
    type Message = MyAppMessage;

    fn new() -> Self {
        Self::default()
    }

    fn title(&self) -> String {
        String::from("My App")
    }

    fn update(&mut self, message: Self::Message) {
        match message {
            MyAppMessage::DoNothing => {}
            MyAppMessage::Update3(b) => self.toggler3 = b,
            MyAppMessage::Update4(b) => self.toggler4 = b,
        }
    }

    fn view(&self) -> iced::Element<Self::Message> {
        column![
            Toggler::new(Some("Construct from struct"), false, |_| MyAppMessage::DoNothing),
            toggler(Some("Construct from function"), false, |_| MyAppMessage::DoNothing),
            toggler(Some("Functional toggler"), self.toggler3, |b| MyAppMessage::Update3(b)),
            toggler(Some("Shorter parameter"), self.toggler4, MyAppMessage::Update4),
            toggler(Some("Larger button"), false, |_| MyAppMessage::DoNothing)
                .size(30),
            toggler(Some("Different font"), false, |_| MyAppMessage::DoNothing)
                .font(Font {
                    family: Family::Fantasy,
                    ..Font::DEFAULT
                }),
            toggler(Some("Larger text"), false, |_| MyAppMessage::DoNothing)
                .text_size(24),
            toggler(Some("Special character 😊"), false, |_| MyAppMessage::DoNothing)
                .text_shaping(Shaping::Advanced),
            toggler(Some("Space between button and text"), false, |_| MyAppMessage::DoNothing)
                .spacing(30),
            toggler(Some("Centered text"), false, |_| MyAppMessage::DoNothing)
                .text_alignment(Horizontal::Center),
        ]
        .into()
    }
}
```

![Toggler](./pic/toggler.png)

:arrow_right: 下一步：[单选按钮](./radio.md)

:blue_book: 返回：[目录](./../README.md)
