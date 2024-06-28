
# 动态更改窗口

我们可以在初始化窗口后使用 [window](https://docs.rs/iced/0.12.1/iced/window/index.html) 模块提供的函数来更改窗口。
例如，使用 [resize](https://docs.rs/iced/0.12.1/iced/window/fn.resize.html) 来调整窗口大小。
这些函数返回 [Command](https://docs.rs/iced/0.12.1/iced/struct.Command.html)，可以作为 [update](https://docs.rs/iced/0.12.1/iced/application/trait.Application.html#tymethod.update) 方法的返回值。
开发人员可能对 [window](https://docs.rs/iced/0.12.1/iced/window/index.html) 模块中的其他 [Commands](https://docs.rs/iced/0.12.1/iced/struct.Command.html) 感兴趣。

[resize](https://docs.rs/iced/0.12.1/iced/window/fn.resize.html) 函数需要我们将要调整大小的窗口的 ID。
在内部，Iced 为首次生成的窗口预留了 [window::Id::MAIN](https://docs.rs/iced/0.12.1/iced/window/struct.Id.html#associatedconstant.MAIN)。

```rust
use iced::{
    executor,
    widget::{button, row, text_input},
    window, Application, Command, Settings, Size,
};

fn main() -> iced::Result {
    MyApp::run(Settings::default())
}

#[derive(Debug, Clone)]
enum MyAppMessage {
    UpdateWidth(String),
    UpdateHeight(String),
    ResizeWindow,
}

struct MyApp {
    width: String,
    height: String,
}

impl Application for MyApp {
    type Executor = executor::Default;
    type Message = MyAppMessage;
    type Theme = iced::Theme;
    type Flags = ();

    fn new(_flags: Self::Flags) -> (Self, iced::Command<Self::Message>) {
        (
            Self {
                width: "1024".into(),
                height: "768".into(),
            },
            Command::none(),
        )
    }

    fn title(&self) -> String {
        String::from("My App")
    }

    fn update(&mut self, message: Self::Message) -> iced::Command<Self::Message> {
        match message {
            MyAppMessage::UpdateWidth(w) => self.width = w,
            MyAppMessage::UpdateHeight(h) => self.height = h,
            MyAppMessage::ResizeWindow => {
                return window::resize(
                    iced::window::Id::MAIN,
                    Size::new(self.width.parse().unwrap(), self.height.parse().unwrap()),
                )
            }
        }
        Command::none()
    }

    fn view(&self) -> iced::Element<Self::Message> {
        row![
            text_input("Width", &self.width).on_input(MyAppMessage::UpdateWidth),
            text_input("Height", &self.height).on_input(MyAppMessage::UpdateHeight),
            button("Resize window").on_press(MyAppMessage::ResizeWindow),
        ]
        .into()
    }
}
```

![动态更改窗口](./pic/changing_the_window_dynamically.png)

:arrow_right: 下一步：[按需关闭窗口](./closing_the_window_on_demand.md)

:blue_book: 返回：[目录](./../README.md)
