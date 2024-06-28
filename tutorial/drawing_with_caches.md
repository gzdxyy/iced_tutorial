
# 使用缓存绘制

之前我们提到了 [Program](https://docs.rs/iced/0.12.1/iced/widget/canvas/trait.Program.html) 有一个 [draw](https://docs.rs/iced/0.12.1/iced/widget/canvas/trait.Program.html#tymethod.draw) 方法，每当对应的 [Canvas](https://docs.rs/iced/0.12.1/iced/widget/canvas/struct.Canvas.html) 需要重新绘制时，就会调用这个方法。
然而，如果 [Canvas](https://docs.rs/iced/0.12.1/iced/widget/canvas/struct.Canvas.html) 的形状保持不变，重新绘制所有这些形状就不太高效。
相反，我们将这些形状的图像存储在 [Cache](https://docs.rs/iced/0.12.1/iced/widget/canvas/struct.Cache.html) 中，当 [Program](https://docs.rs/iced/0.12.1/iced/widget/canvas/trait.Program.html) 的 [draw](https://docs.rs/iced/0.12.1/iced/widget/canvas/trait.Program.html#tymethod.draw) 方法被调用时，我们绘制这个缓存。

为此，我们在应用程序中声明了一个类型为 [Cache](https://docs.rs/iced/0.12.1/iced/widget/canvas/struct.Cache.html) 的字段：

```rust
struct MyApp {
    cache: Cache,
}
```

并对其进行初始化。

```rust
fn new() -> Self {
    Self {
        cache: Cache::new(),
    }
}
```

在 [draw](https://docs.rs/iced/0.12.1/iced/widget/canvas/trait.Program.html#tymethod.draw) 方法中，我们不是创建一个新的 [Frame](https://docs.rs/iced/0.12.1/iced/widget/canvas/enum.Frame.html)：

```rust
let mut frame = Frame::new(renderer, bounds.size());
// ...
vec![frame.into_geometry()]
```

而是使用 [cache.draw](https://docs.rs/iced/0.12.1/iced/widget/canvas/struct.Cache.html#method.draw) 来构建 [Geometry](https://docs.rs/iced/0.12.1/iced/widget/canvas/enum.Geometry.html)。

```rust
let geometry = self.cache.draw(renderer, bounds.size(), |frame| {
    // ...
});

vec![geometry]
```

闭包 `|frame| { /* ... */ }` 仅在 `frame` 的尺寸改变或缓存被明确清除时才被调用。

此外，之前我们为结构体 `MyProgram` 实现了 [Program](https://docs.rs/iced/0.12.1/iced/widget/canvas/trait.Program.html)。
但由于我们需要访问 `MyApp` 的 `cache` 字段，我们必须为 `MyApp` 实现 [Program](https://docs.rs/iced/0.12.1/iced/widget/canvas/trait.Program.html)。

完整代码如下：

```rust
use iced::{
    mouse,
    widget::{
        canvas::{Cache, Geometry, Path, Program, Stroke},
        column, Canvas,
    },
    Alignment, Color, Length, Point, Rectangle, Renderer, Sandbox, Settings, Theme, Vector,
};

fn main() -> iced::Result {
    MyApp::run(Settings::default())
}

struct MyApp {
    cache: Cache,
}

impl Sandbox for MyApp {
    type Message = ();

    fn new() -> Self {
        Self {
            cache: Cache::new(),
        }
    }

    fn title(&self) -> String {
        String::from("My App")
    }

    fn update(&mut self, _message: Self::Message) {}

    fn view(&self) -> iced::Element<Self::Message> {
        column![
            "A Canvas",
            Canvas::new(self).width(Length::Fill).height(Length::Fill)
        ]
        .align_items(Alignment::Center)
        .into()
    }
}

impl<Message> Program<Message> for MyApp {
    type State = ();

    fn draw(
        &self,
        _state: &Self::State,
        renderer: &Renderer,
        _theme: &Theme,
        bounds: Rectangle,
        _cursor: mouse::Cursor,
    ) -> Vec<Geometry> {
        let geometry = self.cache.draw(renderer, bounds.size(), |frame| {
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
        });

        vec![geometry]
    }
}
```

![使用缓存绘制](./pic/drawing_with_caches.png)

:arrow_right: 下一步：[绘制控件](./drawing_widgets.md)

:blue_book: 返回：[目录](./../README.md)
