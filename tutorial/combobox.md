# ComboBox

[ComboBox](https://docs.rs/iced/0.12.1/iced/widget/combo_box/struct.ComboBox.html) 组件代表在多个值中的选择。
值显示在一个下拉菜单中，并且是可以搜索的。
该组件有两种构造方法。
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
    // ... (fields definition)
    // 注意：字段定义部分省略，具体请参考 Rust 代码实现
}

impl Sandbox for MyApp {
    // ... (Sandbox trait implementation)
    // 注意：具体实现部分省略，具体请参考 Rust 代码实现
}

impl MyApp {
    // ... (additional methods if needed)
    // 注意：如果需要，可以添加额外的方法
}

fn main() {
    // main 函数的实现，通常包括运行应用程序的代码
}
```

![ComboBox](./pic/combobox.png)

:arrow_right: 下一步：[滑块和垂直滑块](./slider.md)

:blue_book: 返回：[目录](./../README.md)
