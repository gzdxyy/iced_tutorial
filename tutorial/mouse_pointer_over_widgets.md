
# 鼠标指针悬停在控件上

根据我们控件的需求更改鼠标指针，我们可以使用 [Widget](https://docs.rs/iced/0.12.1/iced/advanced/widget/trait.Widget.html) 的 [mouse_interaction](https://docs.rs/iced/0.12.1/iced/advanced/widget/trait.Widget.html#method.mouse_interaction) 方法。

```rust
fn mouse_interaction(
    &self,
    _state: &Tree,
    layout: Layout<'_>,
    cursor: mouse::Cursor,
    _viewport: &Rectangle,
    _renderer: &Renderer,
) -> mouse::Interaction {
    if cursor.is_over(layout.bounds()) {
        mouse::Interaction::Pointer
    } else {
        mouse::Interaction::Idle
    }
}
```

该方法返回 [Interaction](https://docs.rs/iced/0.12.1/iced/mouse/enum.Interaction.html)，它指定了鼠标指针的类型。
在我们的示例中，当鼠标悬停在控件上时，我们指定 [Interaction::Pointer](https://docs.rs/iced/0.12.1/iced/mouse/enum.Interaction.html#variant.Pointer)。

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

    fn mouse_interaction(
        &self,
        _state: &Tree,
        layout: Layout<'_>,
        cursor: mouse::Cursor,
        _viewport: &Rectangle,
        _renderer: &Renderer,
    ) -> mouse::Interaction {
        if cursor.is_over(layout.bounds()) {
            mouse::Interaction::Pointer
        } else {
            mouse::Interaction::Idle
        }
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

![鼠标指针悬停在控件上](./pic/mouse_pointer_over_widgets.png)

:arrow_right: 下一步：[控件中的文字](./texts_in_widgets.md)

:blue_book: 返回：[目录](./../README.md)
