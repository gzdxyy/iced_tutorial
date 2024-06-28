
# 按需关闭窗口

本教程是在 [上一个教程](./changing_the_window_dynamically.md) 的基础上进行的。
我们使用 [window](https://docs.rs/iced/0.12.1/iced/window/index.html) 模块中的 [close](https://docs.rs/iced/0.12.1/iced/window/fn.close.html) 函数来关闭窗口。
这也是通过返回由 [close](https://docs.rs/iced/0.12.1/iced/window/fn.close.html) 函数获得的 [Command](https://docs.rs/iced/0.12.1/iced/struct.Command.html) 来完成的。

与 [resize](https://docs.rs/iced/0.12.1/iced/window/fn.resize.html) 函数类似，[close](https://docs.rs/iced/0.12.1/iced/window/fn.close.html) 函数也需要窗口的 ID。
我们传递 [window::Id::MAIN](https://docs.rs/iced/0.12.1/iced/window/struct.Id.html#associatedconstant.MAIN) 作为 ID。

```rust
use iced::{
    executor,
    widget::{button, row},
    window, Application, Command, Settings,
};

fn main() -> iced::Result {
    MyApp::run(Settings::default())
}

#[derive(Debug, Clone)]
enum MyAppMessage {
    CloseWindow,
}

struct MyApp;

impl Application for MyApp {
    type Executor = executor::Default;
    type Message = MyAppMessage;
    type Theme = iced::Theme;
    type Flags = ();

    fn new(_flags: Self::Flags) -> (Self, iced::Command<Self::Message>) {
        (Self, Command::none())
    }

    fn title(&self) -> String {
        String::from("My App")
    }

    fn update(&mut self, message: Self::Message) -> iced::Command<Self::Message> {
        match message {
            MyAppMessage::CloseWindow => window::close(window::Id::MAIN),
        }
    }

    fn view(&self) -> iced::Element<Self::Message> {
        row![button("Close window").on_press(MyAppMessage::CloseWindow),].into()
    }
}
```

![按需关闭窗口](./pic/closing_the_window_on_demand.png)

:arrow_right: 下一步：[一些控件的按下/释放](./on_pressed_released_of_some_widgets.md)

:blue_book: 返回：[目录](./../README.md)
