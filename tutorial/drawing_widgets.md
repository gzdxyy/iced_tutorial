
# 绘制控件

除了内置的控件，我们还可以设计自己的自定义控件。
为此，我们需要启用 [advanced](https://docs.rs/crate/iced/0.12.1/features#advanced) 特性。
`Cargo.toml` 文件中的依赖应该如下所示：

```toml
[dependencies]
iced = { version = "0.12.1", features = ["advanced"] }
```

然后，我们需要一个实现 [Widget](https://docs.rs/iced/0.12.1/iced/advanced/widget/trait.Widget.html) 特性的结构体。

```rust
struct MyWidget;

impl<Message, Renderer> Widget<Message, Theme, Renderer> for MyWidget
where
    Renderer: iced::advanced::Renderer,
{
    fn size(&self) -> Size<Length> {
        // ...
    }

    fn layout(
        &self,
        _tree: &mut Tree,
        _renderer: &Renderer,
        _limits: &layout::Limits,
    ) -> layout::Node {
        // ...
    }

    fn draw(
        &self,
        _state: &Tree,
        _renderer: &mut Renderer,
        _theme: &Theme,
        _style: &renderer::Style,
        _layout: Layout<'_>,
        _cursor: mouse::Cursor,
        _viewport: &Rectangle,
    ) {
        // ...
    }
}
```

我们通过 [size](https://docs.rs/iced/0.12.1/iced/advanced/trait.Widget.html#tymethod.size) 和 [layout](https://docs.rs/iced/0.12.1/iced/advanced/widget/trait.Widget.html#tymethod.layout) 方法定义 `MyWidget` 的大小。
目前，我们将宽度和高度设置为 [Length::Shrink](https://docs.rs/iced/0.12.1/iced/enum.Length.html#variant.Shrink)，以告诉布局系统我们为这个控件使用最小的空间。

```rust
fn size(&self) -> Size<Length> {
    Size {
        width: Length::Shrink,
        height: Length::Shrink,
    }
}
```

然后，我们告诉布局系统我们将要为控件使用的确切大小。
在这个例子中，我们的控件大小为 `(100, 100)`。

```rust
fn layout(&self, _renderer: &Renderer, _limits: &layout::Limits) -> layout::Node {
    layout::Node::new([100, 100].into())
}
```

通常，[layout](https://docs.rs/iced/0.12.1/iced/advanced/widget/trait.Widget.html#tymethod.layout) 方法会考虑 [Limits](https://docs.rs/iced/0.12.1/iced/advanced/layout/struct.Limits.html) 参数，这是来自布局系统的限制。
但现在，为了简单起见，我们忽略它。

接下来，我们在 [draw](https://docs.rs/iced/0.12.1/iced/advanced/widget/trait.Widget.html#tymethod.draw) 方法中绘制我们的控件。
我们使用给定的 [Renderer](https://docs.rs/iced/0.12.1/iced/advanced/trait.Renderer.html) 来实现。
可以参考给定的 [Theme](https://docs.rs/iced/0.12.1/iced/enum.Theme.html) 和 [Style](https://docs.rs/iced/0.12.1/iced/advanced/renderer/struct.Style.html) 来获取控件的颜色。

```rust
fn draw(
    &self,
    _state: &Tree,
    renderer: &mut Renderer,
    _theme: &Theme,
    _style: &renderer::Style,
    layout: Layout<'_>,
    _cursor: mouse::Cursor,
    _viewport: &Rectangle,
) {
    renderer.fill_quad(
        Quad {
            bounds: layout.bounds(),
            border: Border {
                color: Color::from_rgb(0.6, 0.8, 1.0),
                width: 1.0,
                radius: 10.0.into(),
            },
            shadow: Shadow::default(),
        },
        Color::from_rgb(0.0, 0.2, 0.4),
    );
}
```

给定的 [Layout](https://docs.rs/iced/0.12.1/iced/advanced/struct.Layout.html) 参数将由布局系统根据我们之前定义的 [size](https://docs.rs/iced/0.12.1/iced/advanced/trait.Widget.html#tymethod.size) 和 [layout](https://docs.rs/iced/0.12.1/iced/advanced/widget/trait.Widget.html#tymethod.layout) 方法自动计算。

为了方便起见，我们可以为 [Element](https://docs.rs/iced/0.12.1/iced/type.Element.html) 实现 `From<MyWidget>`。

```rust
impl<'a, Message, Renderer> From<MyWidget> for Element<'a, Message, Theme, Renderer>
where
    Renderer: iced::advanced::Renderer,
{
    fn from(widget: MyWidget) -> Self {
        Self::new(widget)
    }
}
```

最后，我们可以通过以下代码将控件添加到我们的应用程序中。

```rust
fn view(&self) -> iced::Element<Self::Message> {
    container(MyWidget)
        .width(Length::Fill)
        .height(Length::Fill)
        .center_x()
        .center_y()
        .into()
}
```

注意，没有必要将 `MyWidget` 放在 [Container](https://docs.rs/iced/0.12.1/iced/widget/container/struct.Container.html) 中。
我们可以直接将控件添加到我们的应用程序中。

```rust
fn view(&self) -> iced::Element<Self::Message> {
    MyWidget.into()
}
```

完整代码如下：

```rust
use iced::{
    advanced::{
        layout, mouse,
        renderer::{self, Quad},
        widget::Tree,
        Layout, Widget,
    },
    widget::container,
    Border, Color, Element, Length, Rectangle, Sandbox, Settings, Shadow, Size, Theme,
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
        container(MyWidget)
            .width(Length::Fill)
            .height(Length::Fill)
            .center_x()
            .center_y()
            .into()
    }
}

struct MyWidget;

impl<Message, Renderer> Widget<Message, Theme, Renderer> for MyWidget
where
    Renderer: iced::advanced::Renderer,
{
    fn size(&self) -> Size<Length> {
        Size {
            width: Length::Shrink,
            height: Length::Shrink,
        }
    }

    fn layout(
        &self,
        _tree: &mut Tree,
        _renderer: &Renderer,
        _limits: &layout::Limits,
    ) -> layout::Node {
        layout::Node::new([100, 100].into())
    }

    fn draw(
        &self,
        _state: &Tree,
        renderer: &mut Renderer,
        _theme: &Theme,
        _style: &renderer::Style,
        layout: Layout<'_>,
        _cursor: mouse::Cursor,
        _viewport: &Rectangle,
    ) {
        renderer.fill_quad(
            Quad {
                bounds: layout.bounds(),
                border: Border {
                    color: Color::from_rgb(0.6, 0.8, 1.0),
                    width: 1.0,
                    radius: 10.0.into(),
                },
                shadow: Shadow::default(),
            },
            Color::from_rgb(0.0, 0.2, 0.4),
        );
    }
}

impl<'a, Message, Renderer> From<MyWidget> for Element<'a, Message, Theme, Renderer>
where
    Renderer: iced::advanced::Renderer,
{
    fn from(widget: MyWidget) -> Self {
        Self::new(widget)
    }
}
```

![绘制控件](./pic/drawing_widgets.png)

:arrow_right: 下一步：[从外部更新控件](./updating_widgets_from_outside.md)

:blue_book: 返回：[目录](./../README.md)
