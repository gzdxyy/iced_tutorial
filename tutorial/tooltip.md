# Tooltip

[Tooltip](https://docs.rs/iced/0.12.1/iced/widget/tooltip/struct.Tooltip.html) 组件在鼠标悬停在指定控件上时显示文本。
它有两种构造方法。
它能够改变文本的样式。
我们可以在内部文本周围添加填充。
我们还可以改变工具提示与目标控件之间的间距。
如果允许工具提示超出窗口，那么外面的部分将被剪切。

```rust
use iced::{
    widget::{button, column, tooltip, tooltip::Position, Tooltip},
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
            Tooltip::new(
                button("Mouseover to see the tooltip"),
                "Construct from struct",
                Position::Right
            ),
            tooltip(
                button("Mouseover to see the tooltip"),
                "Construct from function",
                Position::Right
            ),
            tooltip(
                button("Mouseover to see the tooltip"),
                "With padding",
                Position::Right
            )
            .padding(20),
            tooltip(
                button("Mouseover to see the tooltip"),
                "Far away from the widget",
                Position::Right
            )
            .gap(50),
            tooltip(
                button("Mouseover to see the tooltip"),
                "Parts out of the window are clipped",
                Position::Right
            )
            .snap_within_viewport(false),
            tooltip(
                button("Mouseover to see the tooltip"),
                "Follow the cursor",
                Position::FollowCursor
            )
        ]
        .into()
    }
}
```

![Tooltip](./pic/tooltip.png)

:arrow_right: 下一步：[分隔线](./rule.md)

:blue_book: 返回：[目录](./../README.md)
