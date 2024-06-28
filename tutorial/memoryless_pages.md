
# 无记忆页面

[之前的教程](./more_than_one_page.md) 中存在一个问题，即当我们导航到其他页面时，仍然可以访问字段。
这可能带来潜在的安全问题，例如，在其他页面上可能访问到输入的密码。

为了解决这个问题，我们可以使用 [trait 对象](https://doc.rust-lang.org/stable/book/ch17-02-trait-objects.html)。
下面的 `Page` trait 负责单个页面的 [update](https://docs.rs/iced/0.12.1/iced/trait.Sandbox.html#tymethod.update) 和 [view](https://docs.rs/iced/0.12.1/iced/trait.Sandbox.html#tymethod.view)。
在主结构体 `MyApp` 中，我们根据 `MyApp` 中的 `page` 字段派发 [update](https://docs.rs/iced/0.12.1/iced/trait.Sandbox.html#tymethod.update) 和 [view](https://docs.rs/iced/0.12.1/iced/trait.Sandbox.html#tymethod.view) 到相应的页面。
`MyApp` 中的 [update](https://docs.rs/iced/0.12.1/iced/trait.Sandbox.html#tymethod.update) 方法还负责页面的切换。

此外，我们明确区分了不同页面的 `MyAppMessage` 中的消息。

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
    fn view(&self) -> iced::Element<'_, MyAppMessage>;
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

在这个教程中，我们有两个页面，`PageA` 和 `PageB`。
`PageA` 是一个简单的登录表单，而 `PageB` 是一个简单的问候页面。
让我们从 `PageB` 开始。
在其 `update` 方法中，我们只关心 `PageBMessage` 的消息。

```rust
#[derive(Debug, Clone)]
enum PageBMessage {
    ButtonPressed,
}
type Mb = PageBMessage;

struct PageB;

impl PageB {
    fn new() -> Self {
        Self
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
            text("Hello!"),
            button("Log out").on_press(MyAppMessage::PageB(Mb::ButtonPressed)),
        ]
        .into()
    }
}
```

![页面 B](./pic/memoryless_pages_b.png)

在 `PageA` 中，我们在按下登录按钮时检查密码。
如果它是有效的密码，我们切换到 `PageB`。
注意，在我们切换到 `PageB` 后，`PageA`（及其 `password` 字段）将被丢弃。
这确保了密码的安全。

```rust
#[derive(Debug, Clone)]
enum PageAMessage {
    TextChanged(String),
    ButtonPressed,
}
type Ma = PageAMessage;

struct PageA {
    password: String,
}

impl PageA {
    fn new() -> Self {
        Self {
            password: String::new(),
        }
    }
}

impl Page for PageA {
    fn update(&mut self, message: MyAppMessage) -> Option<Box<dyn Page>> {
        if let MyAppMessage::PageA(msg) = message {
            match msg {
                PageAMessage::TextChanged(s) => self.password = s,
                PageAMessage::ButtonPressed => {
                    if self.password == "abc" {
                        return Some(Box::new(PageB::new()));
                    }
                }
            }
        }
        None
    }

    fn view(&self) -> iced::Element<MyAppMessage> {
        column![
            text_input("Password", &self.password)
                .secure(true)
                .on_input(|s| MyAppMessage::PageA(Ma::TextChanged(s))),
            button("Log in").on_press(MyAppMessage::PageA(Ma::ButtonPressed)),
        ]
        .into()
    }
}
```

![页面 A](./pic/memoryless_pages_a.png)

:arrow_right: 下一步：[跨页面传递参数](./passing_parameters_across_pages.md)

:blue_book: 返回：[目录](./../README.md)
