
# 接收任意子控件

由于所有的 [Widget](https://docs.rs/iced/0.12.1/iced/advanced/widget/trait.Widget.html) 都可以转换为 [Element](https://docs.rs/iced_core/0.12.1/iced_core/struct.Element.html)，我们的自定义控件能够接收任意的 [Widget](https://docs.rs/iced/0.12.1/iced/advanced/widget/trait.Widget.html) 作为其子控件。

这次，我们的 `MyWidgetOuter` 在初始化时将接收一个 [Element](https://docs.rs/iced_core/0.12.1/iced_core/struct.Element.html) 作为其内部控件。

```rust
struct MyWidgetOuter<'a, Message, Renderer> {
    inner_widget: Element<'a, Message, Theme, Renderer>,
}

impl<'a, Message, Renderer> MyWidgetOuter<'a, Message, Renderer>
where
    Renderer: iced::advanced::Renderer,
{
    fn new(inner_widget: Element<'a, Message, Theme, Renderer>) -> Self {
        Self { inner_widget }
    }
}
```

在绘制或布局 `inner_widget` 时，我们将使用其 [Widget](https://docs.rs/iced/0.12.1/iced/advanced/widget/trait.Widget.html) 的方法。
然而，`inner_widget` 的类型是 [Element](https://docs.rs/iced_core/0.12.1/iced_core/struct.Element.html)。
因此，我们必须通过 [as_widget](https://docs.rs/iced_core/0.12.1/iced_core/struct.Element.html#method.as_widget) 方法将其转换为 [Widget](https://docs.rs/iced/0.12.1/iced/advanced/widget/trait.Widget.html)。

```rust
fn layout(
    &self,
    tree: &mut Tree,
    renderer: &Renderer,
    limits: &layout::Limits,
) -> layout::Node {
    let mut child_node =
        self.inner_widget
            .as_widget()
            .layout(&mut tree.children[0], renderer, limits);

    let size_of_this_node = child_node.size().expand(Size::new(50., 50.));

    child_node = child_node.align(Alignment::Center, Alignment::Center, size_of_this_node);

    layout::Node::with_children(size_of_this_node, vec![child_node])
}
```

在上述代码中，我们使 `MyWidgetOuter` 的大小相对于其 `inner_widget`。
更准确地说，我们获取 `inner_widget` 的大小并使用 [pad](https://docs.rs/iced_core/0.12.1/iced_core/struct.Size.html#method.pad) 方法将大小设置为 `MyWidgetOuter` 的大小。

然后，在 `MyWidgetOuter` 的 [draw](https://docs.rs/iced/0.12.1/iced/advanced/widget/trait.Widget.html#tymethod.draw) 方法中，我们也绘制 `inner_widget`。

```rust
fn draw(
    &self,
    state: &Tree,
    renderer: &mut Renderer,
    theme: &Theme,
    style: &renderer::Style,
    layout: Layout<'_>,
    cursor: mouse::Cursor,
    viewport: &Rectangle,
) {
    renderer.fill_quad(
        Quad {
            bounds: layout.bounds(),
            border: Border {
                color: Color::from_rgb(0.6, 0.93, 1.0),
                width: 1.0,
                radius: 10.0.into(),
            },
            shadow: Shadow::default(),
        },
        Color::from_rgb(0.0, 0.33, 0.4),
    );

    self.inner_widget.as_widget().draw(
        &state.children[0],
        renderer,
        theme,
        style,
        layout.children().next().unwrap(),
        cursor,
        viewport,
    );
}
```

请注意，我们必须将子状态 `&state.children[0]` 传递给 `inner_widget`，因为匿名控件可能需要其状态信息。

为了使底层系统了解子状态的存在，我们必须明确告诉系统子状态的存在。
否则，[draw](https://docs.rs/iced/0.12.1/iced/advanced/widget/trait.Widget.html#tymethod.draw) 中的 `state.children` 将为空。

```rust
fn children(&self) -> Vec<Tree> {
    vec![Tree::new(self.inner_widget.as_widget())]
}

fn diff(&self, tree: &mut Tree) {
    tree.diff_children(std::slice::from_ref(&self.inner_widget));
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
    widget::{button, container},
    Alignment, Border, Color, Element, Length, Rectangle, Sandbox, Settings, Shadow, Size, Theme,
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
        container(MyWidgetOuter::new(button("Other widget").into()))
            .width(Length::Fill)
            .height(Length::Fill)
            .center_x()
            .center_y()
            .into()
    }
}

struct MyWidgetOuter<'a, Message, Renderer> {
    inner_widget: Element<'a, Message, Theme, Renderer>,
}

impl<'a, Message, Renderer> MyWidgetOuter<'a, Message, Renderer>
where
    Renderer: iced::advanced::Renderer,
{
    fn new(inner_widget: Element<'a, Message, Theme, Renderer>) -> Self {
        Self { inner_widget }
    }
}

impl<Message, Renderer> Widget<Message, Theme, Renderer> for MyWidgetOuter<'_, Message, Renderer>
where
    Renderer: iced::advanced::Renderer,
{
    fn size(&self) -> Size<Length> {
        Size {
            width: Length::Shrink,
            height: Length::Shrink,
        }
    }

    fn diff(&self, tree: &mut Tree) {
        tree.diff_children(std::slice::from_ref(&self.inner_widget));
    }

    fn layout(
        &self,
        tree: &mut Tree,
        renderer: &Renderer,
        limits: &layout::Limits,
    ) -> layout::Node {
        let mut child_node =
            self.inner_widget
                .as_widget()
                .layout(&mut tree.children[0], renderer, limits);

        let size_of_this_node = child_node.size().expand(Size::new(50., 50.));

        child_node = child_node.align(Alignment::Center, Alignment::Center, size_of_this_node);

        layout::Node::with_children(size_of_this_node, vec![child_node])
    }

    fn children(&self) -> Vec<Tree> {
        vec![Tree::new(self.inner_widget.as_widget())]
    }

    fn draw(
        &self,
        state: &Tree,
        renderer: &mut Renderer,
        theme: &Theme,
        style: &renderer::Style,
        layout: Layout<'_>,
        cursor: mouse::Cursor,
        viewport: &Rectangle,
    ) {
        renderer.fill_quad(
            Quad {
                bounds: layout.bounds(),
                border: Border {
                    color: Color::from_rgb(0.6, 0.93, 1.0),
                    width: 1.0,
                    radius: 10.0.into(),
                },
                shadow: Shadow::default(),
            },
            Color::from_rgb(0.0, 0.33, 0.4),
        );

        self.inner_widget.as_widget().draw(
            &state.children[0],
            renderer,
            theme,
            style,
            layout.children().next().unwrap(),
            cursor,
            viewport,
        );
    }
}

impl<'a, Message, Renderer> From<MyWidgetOuter<'a, Message, Renderer>>
    for Element<'a, Message, Theme, Renderer>
where
    Message: 'a,
    Renderer: iced::advanced::Renderer + 'a,
{
    fn from(widget: MyWidgetOuter<'a, Message, Renderer>) -> Self {
        Self::new(widget)
    }
}
```

![接收任意子控件](./pic/taking_any_children.png)

:arrow_right: 下一步：[异步加载图像](./loading_images_asynchronously.md)

:blue_book: 返回：[目录](./../README.md)
