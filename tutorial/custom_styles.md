
# 自定义样式

在[之前的教程](./changing_styles.md)中，我们讨论了如何通过 [theme](https://docs.rs/iced/0.12.1/iced/theme/index.html) 模块中的 [enums](https://doc.rust-lang.org/std/keyword.enum.html) 更改控件的样式。
大多数 [theme](https://docs.rs/iced/0.12.1/iced/theme/index.html) 模块中的 [enums](https://doc.rust-lang.org/std/keyword.enum.html) 支持 `Custom` 变体，例如 [theme::Radio::Custom(...)](https://docs.rs/iced/0.12.1/iced/theme/enum.Radio.html#variant.Custom)。
这个变体将其参数作为 [radio::StyleSheet](https://docs.rs/iced/0.12.1/iced/widget/radio/trait.StyleSheet.html) 特性。
要使用这个变体，我们需要实现该特性（如下代码中的 `RadioStyle` 结构体）。
特性的 [associated type](https://doc.rust-lang.org/stable/book/ch19-03-advanced-traits.html#specifying-placeholder-types-in-trait-definitions-with-associated-types) 应设置为 [iced::Theme](https://docs.rs/iced/0.12.1/iced/enum.Theme.html)。
特性中的方法返回 [radio::Appearance](https://docs.rs/iced/0.12.1/iced/widget/radio/struct.Appearance.html)。
我们可以使用 [theme::Radio::Default](https://docs.rs/iced/0.12.1/iced/theme/enum.Radio.html#variant.Default) 来获取 [radio::Appearance](https://docs.rs/iced/0.12.1/iced/widget/radio/struct.Appearance.html) 的默认值，例如 `style.active(&theme::Radio::Default, is_selected)`。
之后，我们可以根据需要修改默认的 [radio::Appearance](https://docs.rs/iced/0.12.1/iced/widget/radio/struct.Appearance.html)。

```rust
use iced::{
    theme,
    widget::{column, radio},
    Color, Sandbox, Settings,
};

fn main() -> iced::Result {
    MyApp::run(Settings::default())
}

#[derive(Debug, Clone)]
enum MyAppMessage {
    Choose(String),
}

struct MyApp;

impl Sandbox for MyApp {
    type Message = MyAppMessage;

    fn new() -> Self {
        Self
    }

    fn title(&self) -> String {
        String::from("My App")
    }

    fn update(&mut self, _message: Self::Message) {}

    fn view(&self) -> iced::Element<Self::Message> {
        column![
            radio("Choice A", "A", Some("A"), |s| MyAppMessage::Choose(
                s.to_string()
            ))
            .style(theme::Radio::Custom(Box::new(RadioStyle))),
            radio("Choice B", "B", Some("A"), |s| MyAppMessage::Choose(
                s.to_string()
            ))
            .style(theme::Radio::Custom(Box::new(RadioStyle))),
            radio("Choice C", "C", Some("A"), |s| MyAppMessage::Choose(
                s.to_string()
            )),
        ]
        .into()
    }
}

struct RadioStyle;

impl radio::StyleSheet for RadioStyle {
    type Style = iced::Theme;

    fn active(&self, style: &Self::Style, is_selected: bool) -> radio::Appearance {
        radio::Appearance {
            text_color: Some(if is_selected {
                Color::from_rgb(0., 0., 1.)
            } else {
                Color::from_rgb(0.5, 0.5, 0.5)
            }),
            ..style.active(&theme::Radio::Default, is_selected)
        }
    }

    fn hovered(&self, style: &Self::Style, is_selected: bool) -> radio::Appearance {
        style.hovered(&theme::Radio::Default, is_selected)
    }
}
```

![自定义样式](./pic/custom_styles.png)

:arrow_right: 下一步：[超过一个页面](./more_than_one_page.md)

:blue_book: 返回：[目录](./../README.md)
