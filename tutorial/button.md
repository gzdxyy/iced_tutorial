# 按钮

[按钮](https://docs.rs/iced/0.12.1/iced/widget/button/struct.Button.html) 组件支持对按下/触摸事件的反应。
它有两种构造方法。
如果设置了 [on_press](https://docs.rs/iced/0.12.1/iced/widget/button/struct.Button.html#method.on_press) 方法，则按钮处于启用状态，否则处于禁用状态。
我们还可以为按钮文本周围设置填充。

```rust
use iced::{
    widget::{button, column, Button},
    Sandbox, Settings,
};

fn main() -> iced::Result {
    MyApp::run(Settings::default())
}

#[derive(Debug, Clone)]
enum MyAppMessage {
    DoSomething,
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
            Button::new("Disabled button"),
            button("Construct from function"),
            button("Enabled button").on_press(MyAppMessage::DoSomething),
            button("With padding").padding(20),
        ]
        .into()
    }
}
```

![按钮](./pic/button.png)

:arrow_right: 下一步：[文本输入](./text_input.md)

:blue_book: 返回：[目录](./../README.md)
