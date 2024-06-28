
# 批量订阅

本教程是继前两个教程（[键盘事件](./producing_messages_by_keyboard_events.md) 和 [计时器](./producing_messages_by_timers.md)）之后的延续。
我们通过 [Subscription::batch](https://docs.rs/iced/0.12.1/iced/subscription/struct.Subscription.html#method.batch) 函数结合键盘事件和计时器的两个 [Subscriptions](https://docs.rs/iced/0.12.1/iced/subscription/struct.Subscription.html)。

在以下应用程序中，按空格键可以启动或停止计时器。

```rust
use iced::{
    event::{self, Status},
    executor,
    keyboard::{key::Named, Key},
    time::{self, Duration},
    widget::text,
    Application, Command, Event, Settings, Subscription,
};

fn main() -> iced::Result {
    MyApp::run(Settings::default())
}

#[derive(Debug, Clone)]
enum MyAppMessage {
    StartOrStop,
    Update,
}

struct MyApp {
    seconds: u32,
    running: bool,
}

impl Application for MyApp {
    type Executor = executor::Default;
    type Message = MyAppMessage;
    type Theme = iced::Theme;
    type Flags = ();

    fn new(_flags: Self::Flags) -> (Self, iced::Command<Self::Message>) {
        (
            Self {
                seconds: 0,
                running: false,
            },
            Command::none(),
        )
    }

    fn title(&self) -> String {
        String::from("My App")
    }

    fn update(&mut self, message: Self::Message) -> iced::Command<Self::Message> {
        match message {
            MyAppMessage::StartOrStop => self.running = !self.running,
            MyAppMessage::Update => self.seconds += 1,
        }
        Command::none()
    }

    fn view(&self) -> iced::Element<Self::Message> {
        text(self.seconds.to_string()).into()
    }

    fn subscription(&self) -> iced::Subscription<Self::Message> {
        let subscr_key = event::listen_with(|event, status| match (event, status) {
            (
                Event::Keyboard(iced::keyboard::Event::KeyPressed {
                    key: Key::Named(Named::Space),
                    ..
                }),
                Status::Ignored,
            ) => Some(MyAppMessage::StartOrStop),
            _ => None,
        });

        if self.running {
            Subscription::batch(vec![
                subscr_key,
                time::every(Duration::from_secs(1)).map(|_| MyAppMessage::Update),
            ])
        } else {
            subscr_key
        }
    }
}
```

![批量订阅](./pic/batch_subscriptions.png)

:arrow_right: 下一步：[绘制形状](./drawing_shapes.md)

:blue_book: 返回：[目录](./../README.md)
