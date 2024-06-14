# 复选框

[Checkbox](https://docs.rs/iced/0.12.1/iced/widget/checkbox/struct.Checkbox.html) 组件代表一个布尔值。
它有两种构造方法。
如果设置了 [on_toggle](https://docs.rs/iced/0.12.1/iced/widget/struct.Checkbox.html#method.on_toggle) 方法，则复选框处于启用状态，否则处于禁用状态。
它支持对点击和触摸的反应。
它可以改变框和文本的样式。
它还可以改变它们之间的间距。

```rust
use iced::{
    font::Family,
    widget::{
        checkbox,
        checkbox::Icon,
        column,
        text::{LineHeight, Shaping},
        Checkbox,
    },
    Font, Sandbox, Settings,
};

fn main() -> iced::Result {
    MyApp::run(Settings::default())
}

#[derive(Debug, Clone)]
enum MyAppMessage {
    DoNothing,
    Update4(bool),
    Update5(bool),
}

#[derive(Default)]
struct MyApp {
    checkbox4: bool,
    checkbox5: bool,
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
            MyAppMessage::Update4(b) => self.checkbox4 = b,
            MyAppMessage::Update5(b) => self.checkbox5 = b,
        }
    }

    fn view(&self) -> iced::Element<Self::Message> {
        column![
            Checkbox::new("Construct from struct", false),
            checkbox("Construct from function", false),
            checkbox("Enabled checkbox", false).on_toggle(|_| MyAppMessage::DoNothing),
            checkbox("Functional checkbox", self.checkbox4).on_toggle(|b| MyAppMessage::Update4(b)),
            checkbox("Shorter parameter", self.checkbox5).on_toggle(MyAppMessage::Update5),
            checkbox("Larger box", false)
                .on_toggle(|_| MyAppMessage::DoNothing)
                .size(30),
            checkbox("Different icon", true)
                .on_toggle(|_| MyAppMessage::DoNothing)
                .icon(Icon {
                    font: Font::DEFAULT,
                    code_point: '*',
                    size: None,
                    line_height: LineHeight::default(),
                    shaping: Shaping::default()
                }),
            checkbox("Different font", false)
                .on_toggle(|_| MyAppMessage::DoNothing)
                .font(Font {
                    family: Family::Fantasy,
                    ..Font::DEFAULT
                }),
            checkbox("Larger text", false)
                .on_toggle(|_| MyAppMessage::DoNothing)
                .text_size(24),
            checkbox("Special character 😊", false)
                .on_toggle(|_| MyAppMessage::DoNothing)
                .text_shaping(Shaping::Advanced),
            checkbox("Space between box and text", false)
                .on_toggle(|_| MyAppMessage::DoNothing)
                .spacing(30),
        ]
        .into()
    }
}
```

![复选框](./pic/checkbox.png)

:arrow_right: 下一步：[切换器](./toggler.md)

:blue_book: 返回：[目录](./../README.md)
