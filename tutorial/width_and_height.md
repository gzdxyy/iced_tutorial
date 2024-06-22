# 宽度和高度

大多数控件都有 `width` 和 `height` 方法来控制它们的大小。
这些方法接受一个参数 [Length](https://docs.rs/iced/0.12.1/iced/latest/iced/enum.Length.html)。
[Length](https://docs.rs/iced/0.12.1/iced/latest/iced/enum.Length.html) 有四种类型：

* [Shrink](https://docs.rs/iced/0.12.1/iced/latest/iced/enum.Length.html#variant.Shrink)：占用最少的空间。
* [Fill](https://docs.rs/iced/0.12.1/iced/enum.Length.html#variant.Fill)：占用所有剩余的空间。
* [FillPortion](https://docs.rs/iced/0.12.1/iced/enum.Length.html#variant.FillPortion)：与具有 [FillPortion](https://docs.rs/iced/0.12.1/iced/enum.Length.html#variant.FillPortion) 的其他控件相对地占用空间。
* [Fixed](https://docs.rs/iced/0.12.1/iced/enum.Length.html#variant.Fixed)：占用固定大小的空间。

```rust
use iced::{
    widget::{button, column, row},
    Length, Sandbox, Settings,
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
            button("Shrink").width(Length::Shrink),
            button("Fill").width(Length::Fill),
            row![
                button("FillPortion 2").width(Length::FillPortion(2)),
                button("FillPortion 1").width(Length::FillPortion(1)),
            ]
            .spacing(10),
            button("Fixed").width(Length::Fixed(100.)),
            button("Fill (height)").height(Length::Fill),
        ]
        .spacing(10)
        .into()
    }
}
```

![宽度和高度](./pic/width_and_height.png)

:arrow_right: 下一步：[列](./column.md)

:blue_book: 返回：[目录](./../README.md)
