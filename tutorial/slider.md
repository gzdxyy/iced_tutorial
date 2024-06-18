# 滑块和垂直滑块

[Slider](https://docs.rs/iced/0.12.1/iced/widget/slider/struct.Slider.html) 组件表示在给定范围内选择的值。
它有两种构造方法。
它支持对鼠标按下/释放和触摸的反应。
所选值可以对齐到给定的步长。
该组件可以设置为水平或垂直。

```rust
use iced::{
    widget::{column, slider, text, vertical_slider, Slider},
    Sandbox, Settings,
};

fn main() -> iced::Result {
    MyApp::run(Settings::default())
}

#[derive(Debug, Clone)]
enum MyAppMessage {
    DoNothing,
    Update3(u32),
    Update4(u32),
    Update5(u32),
    Update6(u32),
    Update7(u32),
    Release7,
    Update8(u32),
}

struct MyApp {
    value3: u32,
    value4: u32,
    value5: u32,
    value6: u32,
    value7: u32,
    released_value_7: u32,
    value8: u32,
}

impl Sandbox for MyApp {
    type Message = MyAppMessage;

    fn new() -> Self {
        Self {
            value3: 50,
            value4: 50,
            value5: 50,
            value6: 50,
            value7: 50,
            released_value_7: 50,
            value8: 50,
        }
    }

    fn title(&self) -> String {
        String::from("My App")
    }

    fn update(&mut self, message: Self::Message) {
        match message {
            MyAppMessage::DoNothing => {}
            MyAppMessage::Update3(v) => self.value3 = v,
            MyAppMessage::Update4(v) => self.value4 = v,
            MyAppMessage::Update5(v) => self.value5 = v,
            MyAppMessage::Update6(v) => self.value6 = v,
            MyAppMessage::Update7(v) => self.value7 = v,
            MyAppMessage::Release7 => self.released_value_7 = self.value7,
            MyAppMessage::Update8(v) => self.value8 = v,
        }
    }

    fn view(&self) -> iced::Element<Self::Message> {
        column![
            text("Construct from struct"),
            Slider::new(0..=100, 50, |_| MyAppMessage::DoNothing),
            text("Construct from function"),
            slider(0..=100, 50, |_| MyAppMessage::DoNothing),
            text("Functional slider"),
            slider(0..=100, self.value3, |v| MyAppMessage::Update3(v)),
            text("Shorter parameter"),
            slider(0..=100, self.value4, MyAppMessage::Update4),
            text("Different step"),
            slider(0..=100, self.value5, MyAppMessage::Update5).step(10u32),
            text("Different step when a shift key is pressed"),
            slider(0..=100, self.value6, MyAppMessage::Update6).shift_step(10u32),
            text(format!("React to mouse release: {}", self.released_value_7)),
            slider(0..=100, self.value7, MyAppMessage::Update7).on_release(MyAppMessage::Release7),
            text("Press Ctrl (or Command) and click to return to the default value"),
            slider(0..=100, self.value8, MyAppMessage::Update8).default(30u32),
            text("Vertical slider"),
            vertical_slider(0..=100, 50, |_| MyAppMessage::DoNothing),
        ]
        .into()
    }
}
```

![滑块](./pic/slider.png)

:arrow_right: 下一步：[进度条](./progressbar.md)

:blue_book: 返回：[目录](./../README.md)
