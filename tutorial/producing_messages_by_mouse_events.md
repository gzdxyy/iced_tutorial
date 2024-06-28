
# 通过鼠标事件生成消息

要捕获窗口的事件，我们在 [Application](https://docs.rs/iced/0.12.1/iced/application/trait.Application.html) 中实现了 [subscription](https://docs.rs/iced/0.12.1/iced/application/trait.Application.html#method.subscription) 方法。
此方法返回 [Subscription](https://docs.rs/iced/0.12.1/iced/struct.Subscription.html) 结构体，允许我们指定如何处理事件。
我们可以使用 [listen_with](https://docs.rs/iced/0.12.1/iced/event/fn.listen_with.html) 函数来构建 [Subscription](https://docs.rs/iced/0.12.1/iced/struct.Subscription.html)。
[listen_with](https://docs.rs/iced/0.12.1/iced/event/fn.listen_with.html) 函数以一个函数作为输入。
输入函数接受两个参数，[Event](https://docs.rs/iced/0.12.1/iced/event/enum.Event.html) 和 [Status](https://docs.rs/iced/0.12.1/iced/event/enum.Status.html)，并返回 [Option](https://doc.rust-lang.org/std/option/enum.Option.html)\<`MyAppMessage`>，这意味着此函数能够将 [Event](https://docs.rs/iced/0.12.1/iced/event/enum.Event.html) 转换为 `MyAppMessage`。
然后我们在 [update](https://docs.rs/iced/0.12.1/iced/application/trait.Application.html#tymethod.update) 方法中接收转换后的 `MyAppMessage`。

在输入函数中，我们只关心被忽略的事件（即，未被控件处理的事件），通过检查 [Status](https://docs.rs/iced/0.12.1/iced/widget/canvas/event/enum.Status.html) 是否为 [Status::Ignored](https://docs.rs/iced/0.12.1/iced/widget/canvas/event/enum.Status.html#variant.Ignored)。

在本教程中，我们捕获 [Event::Mouse(...)](https://docs.rs/iced/0.12.1/iced/enum.Event.html#variant.Mouse) 和 [Event::Touch(...)](https://docs.rs/iced/0.12.1/iced/enum.Event.html#variant.Touch) 并生成消息。

```rust
use iced::{
    event::{self, Event, Status},
    executor,
    mouse::Event::CursorMoved,
    touch::Event::FingerMoved,
    widget::text,
    Application, Point, Settings,
};

fn main() -> iced::Result {
    MyApp::run(Settings::default())
}

#[derive(Debug, Clone)]
enum MyAppMessage {
    PointUpdated(Point),
}

struct MyApp {
    mouse_point: Point,
}

impl Application for MyApp {
    type Executor = executor::Default;
    type Message = MyAppMessage;
    type Theme = iced::Theme;
    type Flags = ();

    fn new(_flags: Self::Flags) -> (Self, iced::Command<Self::Message>) {
        (
            Self {
                mouse_point: Point::ORIGIN,
            },
            iced::Command::none(),
        )
    }

    fn title(&self) -> String {
        String::from("My App")
    }

    fn update(&mut self, message: Self::Message) -> iced::Command<Self::Message> {
        match message {
            MyAppMessage::PointUpdated(p) => self.mouse_point = p,
        }
        iced::Command::none()
    }

    fn view(&self) -> iced::Element<Self::Message> {
        text(format!("{:?}", self.mouse_point)).into()
    }

    fn subscription(&self) -> iced::Subscription<Self::Message> {
        event::listen_with(|event, status| match (event, status) {
            (Event::Mouse(CursorMoved { position }), Status::Ignored)
            | (Event::Touch(FingerMoved { position, .. }), Status::Ignored) => {
                Some(MyAppMessage::PointUpdated(position))
            }
            _ => None,
        })
    }
}
```

![通过鼠标事件生成消息](./pic/producing_messages_by_mouse_events.png)

:arrow_right: 下一步：[通过键盘事件生成消息](./producing_messages_by_keyboard_events.md)

:blue_book: 返回：[目录](./../README.md)
