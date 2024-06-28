
# 绘制形状

[Canvas](https://docs.rs/iced/0.12.1/iced/widget/canvas/struct.Canvas.html) 是一个帮助我们绘制自由风格形状的控件。
要使用此控件，我们需要启用 [canvas](https://docs.rs/crate/iced/0.12.1/features#canvas) 特性。

```toml
[dependencies]
iced = { version = "0.12.1", features = ["canvas"] }
```

我们使用 [Canvas::new](https://docs.rs/iced/0.12.1/iced/widget/canvas/struct.Canvas.html#method.new) 来获取画布控件。
此函数接受一个 [Program](https://docs.rs/iced/0.12.1/iced/widget/canvas/trait.Program.html) 特性。
我们可以创建一个结构体（比如说 `MyProgram`）来实现这个特性。

```rust
impl<Message> Program<Message> for MyProgram {
    type State = ();

    fn draw(
        &self,
        _state: &Self::State,
        renderer: &Renderer,
        _theme: &Theme,
        bounds: Rectangle,
        _cursor: mouse::Cursor,
    ) -> Vec<Geometry> {
        // ...
    }
}
```

在实现 [Program](https://docs.rs/iced/0.12.1/iced/widget/canvas/trait.Program.html) 特性时，有一个通用数据类型 `Message`。
这有助于我们适应 [Sandbox](https://docs.rs/iced/0.12.1/iced/trait.Sandbox.html) 中的 [Message](https://docs.rs/iced/0.12.1/iced/trait.Sandbox.html#associatedtype.Message)。

关联类型 [State](https://docs.rs/iced/0.12.1/iced/widget/canvas/trait.Program.html#associatedtype.State) 在我们的示例中没有使用，所以我们将其设置为 `()`。

[Program](https://docs.rs/iced/0.12.1/iced/widget/canvas/trait.Program.html) 的关键是 [draw](https://docs.rs/iced/0.12.1/iced/widget/canvas/trait.Program.html#tymethod.draw) 方法。
在该方法中，我们定义要绘制的形状。
我们使用 [Frame](https://docs.rs/iced/0.12.1/iced/widget/canvas/enum.Frame.html) 作为 *笔* 来绘制形状。
例如，我们使用 [Frame](https://docs.rs/iced/0.12.1/iced/widget/canvas/enum.Frame.html) 的 [fill_rectangle](https://docs.rs/iced/0.12.1/iced/widget/canvas/enum.Frame.html#method.fill_rectangle) 方法绘制一个填充的矩形。
或者我们可以描边和填充任何 [Path](https://docs.rs/iced/0.12.1/iced/widget/canvas/struct.Path.html)。
最后，我们使用 [Frame](https://docs.rs/iced/0.12.1/iced/widget/canvas/enum.Frame.html) 的 [into_geometry](https://docs.rs/iced/0.12.1/iced/widget/canvas/enum.Frame.html#method.into_geometry) 方法返回 [Geometry](https://docs.rs/iced/0.12.1/iced/widget/canvas/enum.Geometry.html)，这是 [draw](https://docs.rs/iced/0.12.1/iced/widget/canvas/trait.Program.html#tymethod.draw) 方法所要求的。

```rust
use iced::{
    mouse,
    widget::{
        canvas::{Frame, Geometry, Path, Program, Stroke},
        column, Canvas,
    },
    Alignment, Color, Length, Point, Rectangle, Renderer, Sandbox, Settings, Theme, Vector,
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
            "A Canvas",
            Canvas::new(MyProgram)
                .width(Length::Fill)
                .height(Length::Fill)
        ]
        .align_items(Alignment::Center)
        .into()
    }
}

struct MyProgram;

impl<Message> Program<Message> for MyProgram {
    type State = ();

    fn draw(
        &self,
        _state: &Self::State,
        renderer: &Renderer,
        _theme: &Theme,
        bounds: Rectangle,
        _cursor: mouse::Cursor,
    ) -> Vec<Geometry> {
        let mut frame = Frame::new(renderer, bounds.size());

        frame.fill_rectangle(Point::ORIGIN, bounds.size(), Color::from_rgb(0.0, 0.2, 0.4));

        frame.fill(
            &Path::circle(frame.center(), frame.width().min(frame.height()) / 4.0),
            Color::from_rgb(0.6, 0.8, 1.0),
        );

        frame.stroke(
            &Path::line(
                frame.center() + Vector::new(-250.0, 100.0),
                frame.center() + Vector::new(250.0, -100.0),
            ),
            Stroke {
                style: Color::WHITE.into(),
                width: 50.0,
                ..Default::default()
            },
        );

        vec![frame.into_geometry()]
    }
}
```

![绘制形状](./pic/drawing_shapes.png)

:arrow_right: 下一步：[使用缓存绘制](./drawing_with_caches.md)

:blue_book: 返回：[目录](./../README.md)
