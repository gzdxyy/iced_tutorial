
# 控件中的文字

除了绘制 [Quad](https://docs.rs/iced/latest/iced/advanced/renderer/struct.Quad.html)，我们还可以在我们的控件中绘制文字。

例如，假设我们想要绘制一个名为 `CONTENT` 的字符串切片。

```rust
struct MyWidgetWithText;

impl MyWidgetWithText {
    const CONTENT: &'static str = "  My Widget  ";

    fn new() -> Self {
        Self
    }
}
```

我们使用 [Renderer](https://docs.rs/iced/latest/iced/advanced/text/trait.Renderer.html) 的 [fill_text](https://docs.rs/iced/latest/iced/advanced/text/trait.Renderer.html#tymethod.fill_text) 方法来绘制文字。

```rust
fn draw(
    &self,
    _state: &Tree,
    renderer: &mut Renderer,
    _theme: &Theme,
    _style: &renderer::Style,
    layout: Layout<'_>,
    _cursor: mouse::Cursor,
    viewport: &Rectangle,
) {
    // ...

    let bounds = layout.bounds();

    renderer.fill_text(
        Text {
            content: Self::CONTENT,
            bounds: bounds.size(),
            size: renderer.default_size(),
            line_height: LineHeight::default(),
            font: renderer.default_font(),
            horizontal_alignment: Horizontal::Center,
            vertical_alignment: Vertical::Center,
            shaping: Shaping::default(),
        },
        bounds.center(),
        Color::from_rgb(0.6, 0.8, 1.0),
        *viewport,
    );
}
```

[fill_text](https://docs.rs/iced/latest/iced/advanced/text/trait.Renderer.html#tymethod.fill_text) 方法需要 `Renderer` 类型实现 [iced::advanced::text::Renderer](https://docs.rs/iced/latest/iced/advanced/text/trait.Renderer.html)。
因此，我们需要在控件实现中增加这个要求。

```rust
impl<Message, Renderer> Widget<Message, Theme, Renderer> for MyWidgetWithText
where
    Renderer: iced::advanced::Renderer + iced::advanced::text::Renderer,
```

由于 `Renderer` 类型的要求发生了变化，我们也需要在 `From<MyWidgetWithText>` 转换为 [Element](https://docs.rs/iced/latest/iced/type.Element.html) 时改变要求。

```rust
impl<'a, Message, Renderer> From<MyWidgetWithText> for Element<'a, Message, Theme, Renderer>
where
    Renderer: iced::advanced::Renderer + iced::advanced::text::Renderer,
```

完整代码如下：

```rust
use iced::{
    advanced::{
        layout, mouse,
        renderer::{self, Quad},
        widget::Tree,
        Layout, Text, Widget,
    },
    alignment::{Horizontal, Vertical},
    widget::{
        container,
        text::{LineHeight, Shaping},
    },
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

    fn view(&self) -> iced::Element<'_, Self::Message> {
        container(MyWidgetWithText::new())
            .width(Length::Fill)
            .height(Length::Fill)
            .center_x()
            .center_y()
            .into()
    }
}

struct MyWidgetWithText;

impl MyWidgetWithText {
    const CONTENT: &'static str = "My Widget";

    fn new() -> Self {
        Self
    }
}

impl<Message, Renderer> Widget<Message, Theme, Renderer> for MyWidgetWithText
where
    Renderer: iced::advanced::Renderer + iced::advanced::text::Renderer,
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
        layout::Node::new([200, 100].into())
    }

    fn draw(
        &self,
        _state: &Tree,
        renderer: &mut Renderer,
        _theme: &Theme,
        _style: &renderer::Style,
        layout: Layout<'_>,
        _cursor: mouse::Cursor,
        viewport: &Rectangle,
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

        let bounds = layout.bounds();

        renderer.fill_text(
            Text {
                content: Self::CONTENT,
                bounds: bounds.size(),
                size: renderer.default_size(),
                line_height: LineHeight::default(),
                font: renderer.default_font(),
                horizontal_alignment: Horizontal::Center,
                vertical_alignment: Vertical::Center,
                shaping: Shaping::default(),
            },
            bounds.center(),
            Color::from_rgb(0.6, 0.8, 1.0),
            *viewport,
        );
    }
}

impl<'a, Message, Renderer> From<MyWidgetWithText> for Element<'a, Message, Theme, Renderer>
where
    Renderer: iced::advanced::Renderer + iced::advanced::text::Renderer,
{
    fn from(widget: MyWidgetWithText) -> Self {
        Self::new(widget)
    }
}
```

![控件中的文字](./pic/texts_in_widgets.png)

:arrow_right: 下一步：[自定义背景](./custom_background.md)

:blue_book: 返回：[目录](./../README.md)
