
# 从事件中更新控件

有时，我们希望控件能够自行处理它们的状态。
例如，当接收到 [Event](https://docs.rs/iced/0.12.1/iced/event/enum.Event.html) 时，控件可能会改变其状态。

为此，我们实现了 [Widget](https://docs.rs/iced/0.12.1/iced/advanced/widget/trait.Widget.html) 的 [on_event](https://docs.rs/iced/0.12.1/iced/advanced/widget/trait.Widget.html#method.on_event) 方法。

```rust
fn on_event(
    &mut self,
    _state: &mut Tree,
    event: Event,
    _layout: Layout<'_>,
    _cursor: mouse::Cursor,
    _renderer: &Renderer,
    _clipboard: &mut dyn Clipboard,
    _shell: &mut Shell<'_, Message>,
    _viewport: &Rectangle,
) -> event::Status {
    match event {
        Event::Keyboard(keyboard::Event::KeyPressed {
            key: keyboard::Key::Named(Named::Space),
            ..
        }) => {
            self.highlight = !self.highlight;
            event::Status::Captured
        }
        _ => event::Status::Ignored,
    }
}
```

每次按下空格键时，控件的 `highlight` 字段都会改变。

如果传入 [on_event](https://docs.rs/iced/0.12.1/iced/advanced/widget/trait.Widget.html#method.on_event) 方法的 [Event](https://docs.rs/iced/0.12.1/iced/event/enum.Event.html) 是控件所需的，我们返回 [Status::Captured](https://docs.rs/iced/0.12.1/iced/event/enum.Status.html#variant.Captured)。
否则，我们返回 [Status::Ignored](https://docs.rs/iced/0.12.1/iced/event/enum.Status.html#variant.Ignored) 以告诉系统该事件可以被其他控件使用。

由于我们的控件维护自己的状态，我们不需要从应用程序中传递状态。

```rust
struct MyWidget {
    highlight: bool,
}

impl MyWidget {
    fn new() -> Self {
        Self { highlight: false }
    }
}
```

完整代码如下：

```rust
use iced::{
    advanced::{
        graphics::core::event,
        layout, mouse,
        renderer::{self, Quad},
        widget::Tree,
        Clipboard, Layout, Shell, Widget,
    },
    keyboard::{self, key::Named},
    widget::container,
    Border, Color, Element, Event, Length, Rectangle, Sandbox, Settings, Shadow, Size, Theme,
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
        container(MyWidget::new())
            .width(Length::Fill)
            .height(Length::Fill)
            .center_x()
            .center_y()
            .into()
    }
}

struct MyWidget {
    highlight: bool,
}

impl MyWidget {
    fn new() -> Self {
        Self { highlight: false }
    }
}

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
            if self.highlight {
                Color::from_rgb(0.6, 0.8, 1.0)
            } else {
                Color::from_rgb(0.0, 0.2, 0.4)
            },
        );
    }

    fn on_event(
        &mut self,
        _state: &mut Tree,
        event: Event,
        _layout: Layout<'_>,
        _cursor: mouse::Cursor,
        _renderer: &Renderer,
        _clipboard: &mut dyn Clipboard,
        _shell: &mut Shell<'_, Message>,
        _viewport: &Rectangle,
    ) -> event::Status {
        match event {
            Event::Keyboard(keyboard::Event::KeyPressed {
                key: keyboard::Key::Named(Named::Space),
                ..
            }) => {
                self.highlight = !self.highlight;
                event::Status::Captured
            }
            _ => event::Status::Ignored,
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

按下空格键时，控件颜色在浅色和深色之间切换。

![从事件中更新控件 1](./pic/updating_widgets_from_events_1.png)

![从事件中更新控件 2](./pic/updating_widgets_from_events_2.png)

:arrow_right: 下一步：[生成控件消息](./producing_widget_messages.md)

:blue_book: 返回：[目录](./../README.md)
