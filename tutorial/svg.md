# SVG

[Svg](https://docs.rs/iced/0.12.1/iced/widget/svg/struct.Svg.html) 组件能够显示一个 [SVG](https://en.wikipedia.org/wiki/SVG) 图像。
它有两种构造方法。
我们可以设置如何将图像内容适应到组件边界内。

要使用这个组件，我们必须启用 [svg](https://docs.rs/crate/iced/0.12.1/features#svg) 功能。
`Cargo.toml` 依赖应该如下所示：

```toml
[dependencies]
iced = { version = "0.12.1", features = ["svg"] }
```

让我们将一个名为 `pic.svg` 的 [SVG](https://en.wikipedia.org/wiki/SVG) 图像添加到项目根目录中，即图像的路径为 `my_project/pic.svg`，其中 `my_project` 是我们项目的名称。
文件 `pic.svg` 包含以下内容：

```svg
<svg viewBox="0 0 400 300" xmlns="http://www.w3.org/2000/svg"> 
    <rect width="400" height="300" style="fill:rgb(100,130,160)"/>
    <circle cx="200" cy="150" r="100" style="fill:rgb(180,210,240)"/>
</svg>
```

我们的示例如下：

```rust
use iced::{
    widget::{column, svg, svg::Handle, text, Svg},
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
            text("Construct from struct"),
            Svg::from_path("pic.svg"),
            text("Construct from function"),
            svg(Handle::from_path("pic.svg")),
            text("Different content fit"),
            Svg::from_path("pic.svg").content_fit(ContentFit::None),
        ]
        .into()
    }
}
```

![SVG](./pic/svg.png)

:arrow_right: 下一步：[宽度和高度](./width_and_height.md)

:blue_book: 返回：[目录](./../README.md)
