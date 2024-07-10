下拉选择框（ComboBox）控件表示在多个值中的选择。
这些值显示在一个下拉菜单中，并且可以搜索。
该控件有两种构造方法。
它支持对键盘输入和鼠标选择的反应。
它能够改变字体。
我们可以在内部文本周围添加填充。
我们还可以添加一个可选的图标。

```rust
use iced::{
    font::Family,
    widget::{
        column, combo_box,
        combo_box::State,
        text,
        text_input::{Icon, Side},
        ComboBox,
    },
    Font, Sandbox, Settings,
};

fn main() -> iced::Result {
    MyApp::run(Settings::default())
}

#[derive(Debug, Clone)]
enum MyAppMessage {
    DoNothing,
    Select4(String),
    Select5(String),
    Select6(String),
    Input6(String),
    Select7(String),
    Hover7(String),
    Select8(String),
    Hover8(String),
    Close8,
}

struct MyApp {
    state1: State<u32>,
    state2: State<u32>,
    state3: State<String>,
    state4: State<String>,
    select4: Option<String>,
    state5: State<String>,
    select5: Option<String>,
    state6: State<String>,
    select6: Option<String>,
    input6: String,
    state7: State<String>,
    select7: Option<String>,
    state8: State<String>,
    select8: Option<String>,
    info8: String,
    state9: State<u32>,
    state10: State<u32>,
    state11: State<u32>,
    state12: State<u32>,
}

impl Sandbox for MyApp {
    type Message = MyAppMessage;

    fn new() -> Self {
        Self {
            state1: State::new(vec![]),
            state2: State::new(vec![]),
            state3: State::new(["Aa", "Ab", "Ba", "Bb"].map(|s| s.to_string()).to_vec()),
            state4: State::new(["Aa", "Ab", "Ba", "Bb"].map(|s| s.to_string()).to_vec()),
            select4: None,
            state5: State::new(["Aa", "Ab", "Ba", "Bb"].map(|s| s.to_string()).to_vec()),
            select5: None,
            state6: State::new(["Aa", "Ab", "Ba", "Bb"].map(|s| s.to_string()).to_vec()),
            select6: None,
            input6: "".into(),
            state7: State::new(["Aa", "Ab", "Ba", "Bb"].map(|s| s.to_string()).to_vec()),
            select7: None,
            state8: State::new(["Aa", "Ab", "Ba", "Bb"].map(|s| s.to_string()).to_vec()),
            select8: None,
            info8: "".into(),
            state9: State::new(vec![]),
            state10: State::new(vec![]),
            state11: State::new(vec![]),
            state12: State::new(vec![]),
        }
    }

    fn title(&self) -> String {
        String::from("我的应用")
    }

    fn update(&mut self, message: Self::Message) {
        match message {
            MyAppMessage::DoNothing => {}
            MyAppMessage::Select4(s) => self.select4 = Some(s),
            MyAppMessage::Select5(s) => self.select5 = Some(s),
            MyAppMessage::Select6(s) => self.select6 = Some(s),
            MyAppMessage::Input6(s) => self.input6 = s,
            MyAppMessage::Select7(s) => self.select7 = Some(s),
            MyAppMessage::Hover7(s) => self.select7 = Some(s),
            MyAppMessage::Select8(s) => self.select8 = Some(s),
            MyAppMessage::Hover8(s) => self.select8 = Some(s),
            MyAppMessage::Close8 => self.info8 = "完成！".into(),
        }
    }

    fn view(&self) -> iced::Element<Self::Message> {
        column![
            ComboBox::new(&self.state1, "从结构体构造", None, |_| {
                MyAppMessage::DoNothing
            }),
            combo_box(&self.state2, "从函数构造", None, |_| {
                MyAppMessage::DoNothing
            }),
            combo_box(&self.state3, "带有项目列表", None, |_| {
                MyAppMessage::DoNothing
            }),
            combo_box(
                &self.state4,
                "功能性下拉选择框（按回车或点击选项）",
                self.select4.as_ref(),
                |s| MyAppMessage::Select4(s)
            ),
            combo_box(
                &self.state5,
                "更短的参数（按回车或点击选项）",
                self.select5.as_ref(),
                MyAppMessage::Select5
            ),
            text(&self.input6),
            combo_box(
                &self.state6,
                "响应输入",
                self.select6.as_ref(),
                MyAppMessage::Select6
            )
            .on_input(MyAppMessage::Input6),
            combo_box(
                &self.state7,
                "响应选项悬停",
                self.select7.as_ref(),
                MyAppMessage::Select7
            )
            .on_option_hovered(MyAppMessage::Hover7),
            text(&self.info8),
            combo_box(
                &self.state8,
                "响应关闭菜单",
                self.select8.as_ref(),
                MyAppMessage::Select8
            )
            .on_option_hovered(MyAppMessage::Hover8)
            .on_close(MyAppMessage::Close8),
            combo_box(&self.state9, "不同的字体", None, |_| {
                MyAppMessage::DoNothing
            })
            .font(Font {
                family: Family::Fantasy,
                ..Font::DEFAULT
            }),
            combo_box(&self.state10, "更大的文本", None, |_| {
                MyAppMessage::DoNothing
            })
            .size(24.),
            combo_box(&self.state11, "带填充", None, |_| {
                MyAppMessage::DoNothing
            })
            .padding(20),
            combo_box(&self.state12, "图标", None, |_| MyAppMessage::DoNothing).icon(Icon {
                font: Font::DEFAULT,
                code_point: '\u{2705}',
                size: None,
                spacing: 10.,
                side: Side::Left,
            }),
        ]
        .into()
    }
}
```

![ComboBox](./pic/combobox.png)

:arrow_right: 下一步：[滑块和垂直滑块](./slider.md)

:blue_book: 返回：[目录](./../README.md)
