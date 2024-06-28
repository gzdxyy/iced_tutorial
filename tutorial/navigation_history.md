
# 导航历史

本教程是在 [上一个教程](./passing_parameters_across_pages.md) 的基础上进行的。
[上一个教程](./passing_parameters_across_pages.md) 中介绍的框架可以扩展以处理页面导航历史记录，它能够恢复过去的页面。

我们不是在主结构体 `MyApp` 中保持单个页面，而是可以保持一个页面的 [Vec](https://doc.rust-lang.org/std/vec/struct.Vec.html)。
我们控制 [Vec](https://doc.rust-lang.org/std/vec/struct.Vec.html) 在 [Sandbox](https://docs.rs/iced/0.12.1/iced/trait.Sandbox.html) 的 [update](https://docs.rs/iced/0.12.1/iced/trait.Sandbox.html#tymethod.update) 方法中如何变化。
[Sandbox](https://docs.rs/iced/0.12.1/iced/trait.Sandbox.html) 的 [update](https://docs.rs/iced/0.12.1/iced/trait.Sandbox.html#tymethod.update) 和 `Page` trait 的 `update` 之间的通信是通过自定义的 [enum](https://doc.rust-lang.org/std/keyword.enum.html) `Navigation` 完成的。

```rust
use iced::{
    widget::{button, column, row, text},
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

enum Navigation {
    GoTo(Box<dyn Page>),
    Back,
    None,
}

trait Page {
    fn update(&mut self, message: MyAppMessage) -> Navigation;
    fn view(&self) -> iced::Element<MyAppMessage>;
}

struct MyApp {
    pages: Vec<Box<dyn Page>>,
}

impl Sandbox for MyApp {
    type Message = MyAppMessage;

    fn new() -> Self {
        Self {
            pages: vec![Box::new(PageA::new())],
        }
    }

    fn title(&self) -> String {
        String::from("My App")
    }

    fn update(&mut self, message: Self::Message) {
        let navigation = self.pages.last_mut().unwrap().update(message);
        match navigation {
            Navigation::GoTo(p) => self.pages.push(p),
            Navigation::Back => {
                if self.pages.len() > 1 {
                    self.pages.pop();
                }
            }
            Navigation::None => {}
        }
    }

    fn view(&self) -> iced::Element<Self::Message> {
        self.pages.last().unwrap().view()
    }
}
```

以下是第一类页面的示例：

```rust
#[derive(Debug, Clone)]
enum PageAMessage {
    ButtonPressed,
}
type Ma = PageAMessage;

struct PageA;

impl PageA {
    fn new() -> Self {
        Self
    }
}

impl Page for PageA {
    fn update(&mut self, message: MyAppMessage) -> Navigation {
        if let MyAppMessage::PageA(msg) = message {
            match msg {
                PageAMessage::ButtonPressed => {
                    return Navigation::GoTo(Box::new(PageB::new(1)));
                }
            }
        }
        Navigation::None
    }

    fn view(&self) -> iced::Element<MyAppMessage> {
        column![
            text("Start"),
            button("Next").on_press(MyAppMessage::PageA(Ma::ButtonPressed)),
        ]
        .into()
    }
}
```

![页面 A](./pic/navigation_history_a.png)

以及第二类页面的示例：

```rust
#[derive(Debug, Clone)]
enum PageBMessage {
    BackButtonPressed,
    NextButtonPressed,
}
type Mb = PageBMessage;

struct PageB {
    id: u32,
}

impl PageB {
    fn new(id: u32) -> Self {
        Self { id }
    }
}

impl Page for PageB {
    fn update(&mut self, message: MyAppMessage) -> Navigation {
        if let MyAppMessage::PageB(msg) = message {
            match msg {
                PageBMessage::BackButtonPressed => return Navigation::Back,
                PageBMessage::NextButtonPressed => {
                    return Navigation::GoTo(Box::new(PageB::new(self.id + 1)))
                }
            }
        }
        Navigation::None
    }

    fn view(&self) -> iced::Element<MyAppMessage> {
        column![
            text(self.id.to_string()),
            row![
                button("Back").on_press(MyAppMessage::PageB(Mb::BackButtonPressed)),
                button("Next").on_press(MyAppMessage::PageB(Mb::NextButtonPressed)),
            ],
        ]
        .into()
    }
}
```

![页面 B](./pic/navigation_history_b.png)

:arrow_right: 下一步：[从 Sandbox 到 Application](./from_sandbox_to_application.md)

:blue_book: 返回：[目录](./../README.md)
