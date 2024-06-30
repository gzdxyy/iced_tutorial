
# 带有子控件的控件

自定义控件可以包含另一个自定义控件。

在以下示例中，我们有一个 `MyWidgetOuter` 包含了一个 `MyWidgetInner`。
`MyWidgetInner` 与之前相同，是一个简单的矩形。

```rust
struct MyWidgetInner;

impl<Message, Renderer> Widget<Message, Theme, Renderer> for MyWidgetInner
where
    Renderer: iced::advanced::Renderer,
{
    // ...
}

impl<'a, Message, Renderer> From<MyWidgetInner> for Element<'a, Message, Theme, Renderer>
where
Renderer: iced::advanced::Renderer,
{
    // ...
}
```

`MyWidgetOuter` 包含 `MyWidgetInner`。

```rust
struct MyWidgetOuter {
    inner_widget: MyWidgetInner,
}

impl MyWidgetOuter {
    fn new() -> Self {
        Self {
            inner_widget: MyWidgetInner,
        }
    }
}
```

在 `MyWidgetOuter` 的 [draw](https://docs.rs/iced/12.1/iced/advanced/widget/trait.Widget.html#tymethod.draw) 方法中，我们也绘制 `MyWidgetInner`。

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
            border_radius: 10.0.into(),
            border_width: 1.0,
            border_color: Color::from_rgb(0.6, 0.93, 1.0),
        },
        Color::from_rgb(0.0, 0.33, 0.4),
    );

    let inner_widget = &self.inner_widget as &dyn Widget<Message, Renderer>;
    inner_widget.draw(
        state,
        renderer,
        theme,
        style,
        layout.children().next().unwrap(),
        cursor,
        viewport,
    );
}
```

在 `MyWidgetOuter` 中绘制 `MyWidgetInner` 时，我们需要传递 `MyWidgetInner` 的布局。
这个布局信息可以通过 `layout.children().next().unwrap()` 获得。
[layout.children()](https://docs.rs/iced/0.12.1/iced/advanced/struct.Layout.html#method.children) 是一个 [Iterator](https://doc.rust-lang.org/nightly/core/iter/trait.Iterator.html)，存储了 `MyWidgetOuter` 的所有子布局。

为了使底层系统了解 `MyWidgetOuter` 的子布局，我们必须在 [layout](https://docs.rs/iced/0.12.1/iced/advanced/widget/trait.Widget.html#tymethod.layout) 方法中明确告诉系统。
否则，[draw](https://docs.rs/iced/0.12.1/iced/advanced/widget/trait.Widget.html#tymethod.draw) 方法中的 [layout.children()](https://docs.rs/iced/0.12.1/iced/advanced/struct.Layout.html#method.children) 将为空。

```rust
fn layout(&self, tree: &mut Tree, renderer: &Renderer, limits: &layout::Limits) -> layout::Node {
    let inner_widget = &self.inner_widget as &dyn Widget<Message, Renderer>;
    let mut child_node = inner_widget.layout(tree, renderer, limits);

    let size_of_this_node = Size::new(200., 200.);

    child_node = child_node.align(Alignment::Center, Alignment::Center, size_of_this_node);
    
    layout::Node::with_children(size_of_this_node, vec![child_node])
}
```

我们使用 [Node::with_children](https://docs.rs/iced/0.12.1/iced/advanced/layout/struct.Node.html#method.with_children) 函数将父布局和子布局绑定在一起。

完整代码如下：

```rust
use iced::{
    advanced::{
        layout, mouse,
        renderer::{self, Quad},
        widget::Tree,
        Layout, Widget,
    },
    widget::{container, Theme},
    Alignment, Border, Color, Element, Length, Rectangle, Sandbox, Settings, Shadow, Size,
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
        container(MyWidgetOuter::new())
            .width(Length::Fill)
            .height(Length::Fill)
            .center_x()
            .center_y()
            .into()
    }
}

struct MyWidgetInner;

impl<Message, Renderer> Widget<Message, Theme, Renderer> for MyWidgetInner
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

impl<'a, Message, Renderer> From<MyWidgetInner> for Element<'a, Message, Theme, Renderer>
where
    Renderer: iced::advanced::Renderer,
{
    fn from(widget: MyWidgetInner) -> Self {
        Self::new(widget)
    }
}

struct MyWidgetOuter {
    inner_widget: MyWidgetInner,
}

impl MyWidgetOuter {
    fn new() -> Self {
        Self {
            inner_widget: MyWidgetInner,
        }
    }
}

impl<Message, Renderer> Widget<Message, Theme, Renderer> for MyWidgetOuter
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
        tree: &mut Tree,
        renderer: &Renderer,
        limits: &layout::Limits,
    ) -> layout::Node {
        let inner_widget = &self.inner_widget as &dyn Widget<Message, Theme, Renderer>;
        let mut child_node = inner_widget.layout(tree, renderer, limits);

        let size_of_this_node = Size::new(200., 200.);

        child_node = child_node.align(Alignment::Center, Alignment::Center, size_of_this_node);

        layout::Node::with_children(size_of_this_node, vec![child_node])
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

        let inner_widget = &self.inner_widget as &dyn Widget<Message, Theme, Renderer>;
        inner_widget.draw(
            state,
            renderer,
            theme,
            style,
            layout.children().next().unwrap(),
            cursor,
            viewport,
        );
    }
}

impl<'a, Message, Renderer> From<MyWidgetOuter> for Element<'a, Message, Theme, Renderer>
where
    Renderer: iced::advanced::Renderer,
{
    fn from(widget: MyWidgetOuter) -> Self {
        Self::new(widget)
    }
}
```

![带有子控件的控件](./pic/widgets_with_children.png)

如果 `MyWidgetInner` 接收事件（即，实现 [on_event](https://docs.rs/iced/latest/iced/advanced/trait.Widget.html#method.on_event)），我们必须从 `MyWidgetOuter` 的 [on_event](https://docs.rs/iced/latest/iced/advanced/trait.Widget.html#method.on_event) 方法中调用这个 [on_event](https://docs.rs/iced/latest/iced/advanced/trait.Widget.html#method.on_event) 方法。
这确保了 [Event](https://docs.rs/iced/latest/iced/enum.Event.html) 从 `MyWidgetOuter` 传递到 `MyWidgetInner`。

```rust
fn on_event(
    &mut self,
    state: &mut Tree,
    event: Event,
    layout: Layout<'_>,
    cursor: mouse::Cursor,
    renderer: &Renderer,
    clipboard: &mut dyn Clipboard,
    shell: &mut Shell<'_, Message>,
    viewport: &Rectangle,
) -> event::Status {
    let inner_widget = &mut self.inner_widget as &mut dyn Widget<Message, Theme, Renderer>;

    inner_widget.on_event(
        state,
        event,
        layout.children().next().unwrap(),
        cursor,
        renderer,
        clipboard,
        shell,
        viewport,
    )
}
```

:arrow_right: 下一步：[接收任意子控件](./taking_any_children.md)

:blue_book: 返回：[目录](./../README.md)
