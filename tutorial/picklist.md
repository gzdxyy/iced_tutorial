# PickList

[PickList](https://docs.rs/iced/0.12.1/iced/widget/pick_list/struct.PickList.html) 组件代表在多个值中的选择。
它有两种构造方法。
它支持对选项选择以及菜单打开/关闭的反应。
当尚未选择选项时，可以设置一个占位符。
它能够改变文本的样式。
我们可以在内部文本周围添加填充。
我们还可以改变手柄的图标。

```rust
use iced::{
    font::Family,
    widget::{column, pick_list, pick_list::Handle, row, text, text::Shaping, PickList},
    Font, Pixels, Sandbox, Settings,
};

fn main() -> iced::Result {
    MyApp::run(Settings::default())
}

#[derive(Debug, Clone)]
enum MyAppMessage {
    DoNothing,
    Update3(String),
    Open10,
    Close11,
}

struct MyApp {
    pick_list_3: Option<String>,
    info_10: String,
    info_11: String,
}

impl Sandbox for MyApp {
    type Message = MyAppMessage;

    fn new() -> Self {
        Self {
            pick_list_3: Some("Functional pick list".into()),
            info_10: "".into(),
            info_11: "".into(),
        }
    }

    fn title(&self) -> String {
        String::from("My App")
    }

    fn update(&mut self, message: Self::Message) {
        match message {
            MyAppMessage::DoNothing => {}
            MyAppMessage::Update3(s) => self.pick_list_3 = Some(s),
            MyAppMessage::Open10 => self.info_10 = "Open".into(),
            MyAppMessage::Close11 => self.info_11 = "Close".into(),
        }
    }

    fn view(&self) -> iced::Element<Self::Message> {
        column![
            PickList::new(
                vec!["Construct from struct"], 
                Some("Construct from struct"), 
                |_| MyAppMessage::DoNothing
            ),
            pick_list(
                vec!["Construct from function"], 
                Some("Construct from function"), 
                |_| MyAppMessage::DoNothing
            ),
            pick_list(
                ["Functional pick list", "Other choices 1", "Other choices 2"]
                    .iter().map(|s| s.to_string()).collect(),
                self.pick_list_3.clone(),
                |s| MyAppMessage::Update3(s)
            ),
            pick_list(vec!["A", "B", "C"], None, |_| {
                MyAppMessage::DoNothing
            })
            .placeholder("Placeholder"),
            pick_list(vec!["Different font"], Some("Different font"), |_| {
                MyAppMessage::DoNothing
            })
            .font(Font {
                family: Family::Fantasy,
                ..Font::DEFAULT
            }),
            pick_list(vec!["Larger text"], Some("Larger text"), |_| {
                MyAppMessage::DoNothing
            })
            .text_size(24),
            pick_list(
                vec!["Special character 😊"], 
                Some("Special character 😊"), 
                |_| MyAppMessage::DoNothing
            )
            .text_shaping(Shaping::Advanced),
            pick_list(vec!["With padding"], Some("With padding"), |_| {
                MyAppMessage::DoNothing
            })
            .padding(20),
            pick_list(vec!["Different handle"], Some("Different handle"), |_| {
                MyAppMessage::DoNothing
            })
            .handle(Handle::Arrow {
                size: Some(Pixels(24.))
            }),
            row![
                pick_list(vec!["Respond to open"], Some("Respond to open"), |_| {
                    MyAppMessage::DoNothing
                })
                .on_open(MyAppMessage::Open10),
                text(&self.info_10),
            ],
            row![
                pick_list(vec!["Respond to close"], Some("Respond to close"), |_| {
                    MyAppMessage::DoNothing
                })
                .on_close(MyAppMessage::Close11),
                text(&self.info_11),
            ],
        ]
        .into()
    }
}
```

![PickList](./pic/picklist.png)

:arrow_right: 下一步：[下拉框](./combobox.md)

:blue_book: 返回：[目录](./../README.md)
