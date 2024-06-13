# Sandbox Trait 说明

[Sandbox](https://docs.rs/iced/0.12.1/iced/trait.Sandbox.html) 的工作方式如下。

![Sandbox Trait](./pic/explanation_of_sandbox_trait.png)

以下代码也展示了这一点。

```rust
#[derive(Debug, Clone)]
enum Message {
    // ... （消息类型定义）
}

struct MyApp {
    // ... （应用状态的一些字段）
}

impl Sandbox for MyApp {
    type Message = Message; // 消息类型

    fn new() -> Self {
        Self {
            // ... （初始化状态）
        }
    }

    fn title(&self) -> String {
        // 窗口的标题
    }

    fn update(&mut self, message: Self::Message) {
        match message {
            // 更新逻辑
        }
    }

    fn view(&self) -> iced::Element<Self::Message> {
        // 视图逻辑
    }
}
```

:arrow_right: 下一步：[添加控件](./adding_widgets.md)

:blue_book: 返回：[目录](./../README.md)
