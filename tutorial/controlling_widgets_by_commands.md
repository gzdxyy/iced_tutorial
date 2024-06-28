
# 通过命令控制控件

我们可以使用 [Command](https://docs.rs/iced/0.12.1/iced/struct.Command.html) 来控制控件。
一些控件有 [函数](https://doc.rust-lang.org/stable/book/ch03-03-how-functions-work.html) 来改变它们的行为。
这些 [函数](https://doc.rust-lang.org/stable/book/ch03-03-how-functions-work.html) 位于它们各自的模块中。
例如，[focus](https://docs.rs/iced/0.12.1/iced/widget/text_input/fn.focus.html) 是 [text_input](https://docs.rs/iced/0.12.1/iced/widget/text_input/index.html) 模块中的一个函数，它使 [TextInput](https://docs.rs/iced/0.12.1/iced/widget/text_input/struct.TextInput.html) 获得焦点。
这个函数接受一个参数 [text_input::Id](https://docs.rs/iced/0.12.1/iced/widget/text_input/struct.Id.html)，可以通过 [TextInput](https://docs.rs/iced/0.12.1/iced/widget/text_input/struct.TextInput.html) 的 [id](https://docs.rs/iced/0.12.1/iced/widget/text_input/struct.TextInput.html#method.id) 方法指定。
该函数返回一个 [Command](https://docs.rs/iced/0.12.1/iced/struct.Command.html)，我们可以在 [Application](https://docs.rs/iced/0.12.1/iced/application/trait.Application.html) 的 [update](https://docs.rs/iced/0.12.1/iced.rs/iced/application/trait.Application.html#tymethod.update) 或 [new](https://docs.rs/iced/0.12.1/iced/application/trait.Application.html#tymethod.new) 方法中返回这个 [Command](https://docs.rs/iced/0.12.1/iced/struct.Command.html)。

```rust
use iced::{
    executor,
    widget::{button, column, text_input},
    Application, Command, Settings,
};

fn main() -> iced::Result {
    MyApp::run(Settings::default())
}

const MY_TEXT_ID: &str = "my_text";

#[derive(Debug, Clone)]
enum MyAppMessage {
    EditText,
    UpdateText(String),
}

struct MyApp {
    some_text: String,
}

impl Application for MyApp {
    type Executor = executor::Default;
    type Message = MyAppMessage;
    type Theme = iced::Theme;
    type Flags = ();

    fn new(_flags: Self::Flags) -> (Self, iced::Command<Self::Message>) {
        (
            Self {
                some_text: String::new(),
            },
            Command::none(),
        )
    }

    fn title(&self) -> String {
        String::from("My App")
    }

    fn update(&mut self, message: Self::Message) -> iced::Command<Self::Message> {
        match message {
            MyAppMessage::EditText => return text_input::focus(text_input::Id::new(MY_TEXT_ID)),
            MyAppMessage::UpdateText(s) => self.some_text = s,
        }
        Command::none()
    }

    fn view(&self) -> iced::Element<Self::Message> {
        column![
            button("Edit text").on_press(MyAppMessage::EditText),
            text_input("", &self.some_text)
                .id(text_input::Id::new(MY_TEXT_ID))
                .on_input(MyAppMessage::UpdateText),
        ]
        .into()
    }
}
```

![通过命令控制控件](./pic/controlling_widgets_by_commands.png)

:arrow_right: 下一步：[批量命令](./batch_commands.md)

:blue_book: 返回：[目录](./../README.md)
