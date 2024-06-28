
# 跨页面传递参数

本教程是在 [上一个教程](./memoryless_pages.md) 的基础上进行的。
我们使用相同的 `Page` trait 和 `MyApp` 结构体。

```rust
use iced::{
    widget::{button, column, text, text_input},
    Sandbox, Settings,
};

fn main() -> iced::Result {
    MyApp::run(Settings::default())
}

#[derive(Debug, Clone)]
enum MyAppMessage {
    PageA(PageAMessage),
    PageB(PageBMessage),
}

trait Page {
    fn update(&mut self, message: MyAppMessage) -> Option<Box<dyn Page>>;
    fn view(&self) -> iced::Element<MyAppMessage>;
}

struct MyApp {
    page: Box<dyn Page>,
}

impl Sandbox for MyApp {
    type Message = MyAppMessage;

    fn new() -> Self {
        Self {
            page: Box::new(PageA::new()),
        }
    }

    fn title(&self) -> String {
        String::from("My App")
    }

    fn update(&mut self, message: Self::Message) {
        let page = self.page.update(message);
        if let Some(p) = page {
            self.page = p;
        }
    }

    fn view(&self) -> iced::Element<Self::Message> {
        self.page.view()
    }
}
```

对于 `PageA`（登录表单），我们有一个用于输入名字的 [TextInput](https://docs.rs/iced/0.12.1/iced/widget/struct.TextInput.html) 和一个提交的 [Button](https://docs.rs/iced/0.12.1/iced/widget/struct.Button.html)。
当我们按下提交按钮时，我们将 `PageA` 的 `name` 字段传递给 `PageB` 的 `new` 函数。

```rust
#[derive(Debug, Clone)]
enum PageAMessage {
    TextChanged(String),
    ButtonPressed,
}
type Ma = PageAMessage;

struct PageA {
    name: String,
}

impl PageA {
    fn new() -> Self {
        Self {
            name: String::new(),
        }
    }
}

impl Page for PageA {
    fn update(&mut self, message: MyAppMessage) -> Option<Box<dyn Page>> {
        if let MyAppMessage::PageA(msg) = message {
            match msg {
                PageAMessage::TextChanged(s) => self.name = s,
                PageAMessage::ButtonPressed => {
                    return Some(Box::new(PageB::new(self.name.clone())))
                }
            }
        }
        None
    }

    fn view(&self) -> iced::Element<MyAppMessage> {
        column![
            text_input("Name", &self.name).on_input(|s| MyAppMessage::PageA(Ma::TextChanged(s))),
            button("Log in").on_press(MyAppMessage::PageA(Ma::ButtonPressed)),
        ]
        .into()
    }
}
```

![页面 A](./pic/passing_parameters_across_pages_a.png)

在 `PageB` 中，我们从 `new` 函数接收名字，并在 `view` 中显示这个名字。

```rust
#[derive(Debug, Clone)]
enum PageBMessage {
    ButtonPressed,
}
type Mb = PageBMessage;

struct PageB {
    name: String,
}

impl PageB {
    fn new(name: String) -> Self {
        Self { name }
    }
}

impl Page for PageB {
    fn update(&mut self, message: MyAppMessage) -> Option<Box<dyn Page>> {
        if let MyAppMessage::PageB(msg) = message {
            match msg {
                PageBMessage::ButtonPressed => return Some(Box::new(PageA::new())),
            }
        }
        None
    }

    fn view(&self) -> iced::Element<MyAppMessage> {
        column![
            text(format!("Hello {}!", self.name)),
            button("Log out").on_press(MyAppMessage::PageB(Mb::ButtonPressed)),
        ]
        .into()
    }
}
```

![页面 B](./pic/passing_parameters_across_pages_b.png)

:arrow_right: 下一步：[导航历史](./navigation_history.md)

:blue_book: 返回：[目录](./../README.md)
