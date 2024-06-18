# 图像

[Image](https://docs.rs/iced/0.12.1/iced/widget/image/struct.Image.html) 组件能够显示一个图像。
它有两种构造方法。
我们可以设置如何将图像内容适应到组件边界内。

要使用该组件，我们必须启用 [image](https://docs.rs/crate/iced/0.12.1/features#image) 特性。
`Cargo.toml` 依赖应该如下所示：

```toml
[dependencies]
iced = { version = "0.12.1", features = ["image"] }
```

假设我们在项目根目录下有一个名为 `ferris.png` 的图像，即图像的路径为 `my_project/ferris.png`，其中 `my_project` 是我们项目的名称。

```rust
use iced::{
    widget::{column, image, text, Image},
    ContentFit, Sandbox, Settings,
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
            text("通过结构体构造"),
            Image::new("ferris.png"),
            text("通过函数构造"),
            image("ferris.png"),
            text("不同的内容适应"),
            image("ferris.png").content_fit(ContentFit::Cover),
        ]
        .into()
    }
}
```

![图像](./pic/image.png)

:arrow_right: 下一步：[SVG](./svg.md)

:blue_book: 返回：[目录](./../README.md)
