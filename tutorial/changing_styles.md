
# 更改样式

大多数控件支持 `style` 方法来更改它们的风格。
这些方法接受来自 [theme](https://docs.rs/iced/0.12.1/iced/theme/index.html) 模块的 [enums](https://doc.rust-lang.org/std/keyword.enum.html) 作为参数。
例如，[widget::Text](https://docs.rs/iced/0.12.1/iced/widget/type.Text.html) 接受 [theme::Text](https://docs.rs/iced/0.12.1/iced/theme/enum.Text.html) 作为其 [style](https://docs.rs/iced/0.12.1/iced/advanced/widget/struct.Text.html#method.style) 方法的参数，而 [widget::Button](https://docs.rs/iced/0.12.1/iced/widget/struct.Button.html) 接受 [theme::Button](https://docs.rs/iced/0.12.1/iced/theme/enum.Button.html) 作为其 [style](https://docs.rs/iced/0.12.1/iced/widget/struct.Button.html#method.style) 方法的参数。

由于 [theme::Text](https://docs.rs/iced/0.12.1/iced/theme/enum.Text.html) 实现了 [From\<Color>](https://docs.rs/iced/0.12.1/iced/theme/enum.Text.html#impl-From%3CColor%3E-for-Text)，我们还可以直接使用 [Color](https://docs.rs/iced/0.12.1/iced/struct.Color.html) 结构体作为 [widget::Text](https://docs.rs/iced/0.12.1/iced/widget/type.Text.html) 的 [style](https://docs.rs/iced/0.12.1/iced/advanced/widget/struct.Text.html#method.style) 方法。

```rust
use iced::{
    theme,
    widget::{button, column, row, text},
    Color, Sandbox, Settings,
};

fn main() -> iced::Result {
    MyApp::run(Settings::default())
}

#[derive(Debug, Clone)]
enum MyAppMessage {
    DummyMessage,
}

struct MyApp;

impl Sandbox for MyApp {
    type Message = MyAppMessage;

    fn new() -> Self {
        Self
    }

    fn title(&self) -> String {
        String::from("My App")
    }

    fn update(&mut self, _message: Self::Message) {}

    fn view(&self) -> iced::Element<Self::Message> {
        column![
            text("Ready?")
                .style(Color::from_rgb(1., 0.6, 0.2)),
            row![
                button("Cancel")
                    .style(theme::Button::Secondary)
                    .on_press(MyAppMessage::DummyMessage),
                button("Go!~~")
                    .style(theme::Button::Primary)
                    .on_press(MyAppMessage::DummyMessage),
            ],
        ]
        .into()
    }
}
```

![更改样式](./pic/changing_styles.png)

:arrow_right: 下一步：[自定义样式](./custom_styles.md)

:blue_book: 返回：[目录](./../README.md)
