
# 响应控件的按下/释放事件

如果我们只关心鼠标按下或释放事件，我们可以使用 [MouseArea](https://docs.rs/iced/0.12.1/iced/widget/struct.MouseArea.html)。
[MouseArea](https://docs.rs/iced/0.12.1/iced/widget/struct.MouseArea.html) 为放入其中的控件提供了鼠标按下/释放事件的感知能力，即使该控件没有内置对这些事件的支持。
例如，我们可以让一个 [Text](https://docs.rs/iced/0.12.1/iced/widget/type.Text.html) 响应鼠标按下/释放事件。

```rust
use iced::{widget::mouse_area, Sandbox, Settings};

fn main() -> iced::Result {
    MyApp::run(Settings::default())
}

#[derive(Debug, Clone)]
enum MyAppMessage {
    Pressed,
    Released,
}

struct MyApp {
    state: String,
}

impl Sandbox for MyApp {
    type Message = MyAppMessage;

    fn new() -> Self {
        Self {
            state: "Start".into(),
        }
    }

    fn title(&self) -> String {
        String::from("My App")
    }

    fn update(&mut self, message: Self::Message) {
        match message {
            MyAppMessage::Pressed => self.state = "Pressed".into(),
            MyAppMessage::Released => self.state = "Released".into(),
        }
    }

    fn view(&self) -> iced::Element<Self::Message> {
        mouse_area(self.state.as_str())
            .on_press(MyAppMessage::Pressed)
            .on_release(MyAppMessage::Released)
            .into()
    }
}
```

除了 [on_press](https://docs.rs/iced/0.12.1/iced/widget/struct.MouseArea.html#method.on_press) 和 [on_release](https://docs.rs/iced/0.12.1/iced/widget/struct.MouseArea.html#method.on_release) 方法外，[MouseArea](https://docs.rs/iced/0.12.1/iced/widget/struct.MouseArea.html) 还支持 [on_middle_press](https://docs.rs/iced/0.12.1/iced/widget/struct.MouseArea.html#method.on_middle_press)、[on_right_press](https://docs.rs/iced/0.12.1/iced/widget/struct.MouseArea.html#method.on_right_press) 等。

当鼠标按下时：

![响应控件的按下/释放事件 A](./pic/on_pressed_released_of_some_widgets_a.png)

当鼠标释放时：

![响应控件的按下/释放事件 B](./pic/on_pressed_released_of_some_widgets_b.png)

:arrow_right: 下一步：[通过鼠标事件生成消息](./producing_messages_by_mouse_events.md)

:blue_book: 返回：[目录](./../README.md)
