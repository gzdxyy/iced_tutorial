# 进度条

[ProgressBar](https://docs.rs/iced/0.12.1/iced/widget/progress_bar/struct.ProgressBar.html) 组件表示给定范围内的值。
它有两种构造方法。

```rust
use iced::{
    widget::{column, progress_bar, text, ProgressBar},
    Sandbox, Settings,
};

fn main() -> iced::Result {
    MyApp::run(Settings::default())
}

struct MyApp;

impl Sandbox for MyApp {
    type Message = ();

    fn new() -> Self {
        Self
    }

    fn title(&self) -> String {
        String::from("My App")
    }

    fn update(&mut self, _message: Self::Message) {}

    fn view(&self) -> iced::Element<Self::Message> {
        column![
            text("Construct from struct"),
            ProgressBar::new(0.0..=100.0, 50.),
            text("Construct from function"),
            progress_bar(0.0..=100.0, 30.),
        ]
        .into()
    }
}
```

![进度条](./pic/progressbar.png)

:arrow_right: 下一步：[工具提示](./tooltip.md)

:blue_book: 返回：[目录](./../README.md)
