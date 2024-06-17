# 单选按钮

[Radio](https://docs.rs/iced/0.12.1/iced/widget/radio/struct.Radio.html) 组件代表在多个值中的选择。
它有两种构造方法。
它支持对点击和触摸的反应。
它可以改变按钮和文本的样式。
它还可以改变它们之间的间距。

```rust
use iced::{
    font::Family,
    widget::{column, radio, row, text, text::Shaping, Radio},
    Font, Sandbox, Settings,
};

fn main() -> iced::Result {
    MyApp::run(Settings::default())
}

#[derive(Debug, Clone)]
enum MyAppMessage {
    DoNothing,
    Update3(u32),
    Update4(String),
}

struct MyApp {
    radio3: Option<u32>,
    radio4: Option<String>,
}

impl Sandbox for MyApp {
    type Message = MyAppMessage;

    fn new() -> Self {
        Self {
            radio3: Some(1),
            radio4: Some("a".into()),
        }
    }

    fn title(&self) -> String {
        String::from("My App")
    }

    fn update(&mut self, message: Self::Message) {
        match message {
            MyAppMessage::DoNothing => {}
            MyAppMessage::Update3(i) => self.radio3 = Some(i),
            MyAppMessage::Update4(s) => self.radio4 = Some(s),
        }
    }

    fn view(&self) -> iced::Element<Self::Message> {
        column![
            Radio::new("Construct from struct", 0, &None, |_| {
                MyAppMessage::DoNothing
            }),
            radio("Construct from function", 0, &None, |_| {
                MyAppMessage::DoNothing
            }),
            row![
                text("Functional radio"),
                radio("A", 1, &self.radio3, |i| MyAppMessage::Update3(i)),
                radio("B", 2, &self.radio3, |i| MyAppMessage::Update3(i)),
                radio("C", 3, &self.radio3, |i| MyAppMessage::Update3(i)),
            ],
            row![
                text("Radio of String values"),
                radio("A", &"a".to_string(), &self.radio4, |s| {
                    MyAppMessage::Update4(s.clone())
                }),
                radio("B", &"b".to_string(), &self.radio4, |s| {
                    MyAppMessage::Update4(s.clone())
                }),
                radio("C", &"c".to_string(), &self.radio4, |s| {
                    MyAppMessage::Update4(s.clone())
                }),
            ],
            radio("Larger button", 0, &None, |_| MyAppMessage::DoNothing)
                .size(40),
            radio("Different font", 0, &None, |_| MyAppMessage::DoNothing)
                .font(Font {
                    family: Family::Fantasy,
                    ..Font::DEFAULT
                }),
            radio("Larger text", 0, &None, |_| MyAppMessage::DoNothing)
                .text_size(24),
            radio("Special character 😊", 0, &None, |_| {
                MyAppMessage::DoNothing
            })
            .text_shaping(Shaping::Advanced),
            radio("Space between button and text", 0, &None, |_| {
                MyAppMessage::DoNothing
            })
            .spacing(30),
        ]
        .into()
    }
}
```

![单选按钮](./pic/radio.png)

:arrow_right: 下一步：[选择列表](./picklist.md)

:blue_book: 返回：[目录](./../README.md)
