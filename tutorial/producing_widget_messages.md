
# 生成控件消息

我们的自定义控件能够发送 [Message](https://docs.rs/iced/0.12.1/iced/trait.Sandbox.html#associatedtype.Message)。

为此，我们需要在控件中存储将要发送的 [Message](https://docs.rs/iced/0.12.1/iced/trait.Sandbox.html#associatedtype.Message)。

```rust
struct MyWidget<Message> {
    pressed_message: Message,
}

impl<Message> MyWidget<Message> {
    fn new(pressed_message: Message) -> Self {
        Self { pressed_message }
    }
}
```

我们为 `MyWidget` 使用了一个泛型 `Message`，这样 `MyWidget` 中发送的 `pressed_message` 将与 [Sandbox](https://docs.rs/iced/0.12.1/iced/trait.Sandbox.html) 的关联类型 [Message](https://docs.rs/iced/0.12.1/iced/trait.Sandbox.html#associatedtype.Message) 匹配。

当控件被按下时，将发送消息 `pressed_message`。

```rust
fn on_event(
    &mut self,
    _state: &mut Tree,
    event: Event,
    layout: Layout<'_>,
    cursor: mouse::Cursor,
    _renderer: &Renderer,
    _clipboard: &mut dyn Clipboard,
    shell: &mut Shell<'_, Message>,
    _viewport: &Rectangle,
) -> event::Status {
    if cursor.is_over(layout.bounds()) {
        match event {
            Event::Mouse(mouse::Event::ButtonPressed(_)) => {
                shell.publish(self.pressed_message.clone());
                event::Status::Captured
            }
            _ => event::Status::Ignored,
        }
    } else {
        event::Status::Ignored
    }
}
```

我们使用 `shell.publish(self.pressed_message.clone())` 将 `pressed_message` 发送到我们的应用程序。
为确保鼠标按下事件发生在控件的范围内，我们使用 `cursor.is_over(layout.bounds())` 检查鼠标位置，并匹配 `event` 到 `Event::Mouse(mouse::Event::ButtonPressed(_))` 检查鼠标按钮状态。

最后，我们将我们的消息 [Message](https://docs.rs/iced/0.12.1/iced/trait.Sandbox.html#associatedtype.Message) 传递给控件。

```rust
#[derive(Debug, Clone)]
enum MyMessage {
    MyWidgetPressed,
}

// ...

impl Sandbox for MyApp {
    type Message = MyMessage;

    // ...

    fn view(&self) -> iced::Element<'_, Self::Message> {
        container(
            column![
                MyWidget::new(MyMessage::MyWidgetPressed),
                // ...
            ]
            // ...
        )
        // ...
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
    widget::{column, container, text},
    Border, Color, Element, Event, Length, Rectangle, Sandbox, Settings, Shadow, Size, Theme,
};

fn main() -> iced::Result {
    MyApp::run(Settings::default())
}

#[derive(Debug, Clone)]
enum MyMessage {
    MyWidgetPressed,
}

struct MyApp {
    count: u32,
}

impl Sandbox for MyApp {
    type Message = MyMessage;

    fn new() -> Self {
        Self { count: 0 }
    }

    fn title(&self) -> String {
        String::from("My App")
    }

    fn update(&mut self, message: Self::Message) {
        match message {
            MyMessage::MyWidgetPressed => self.count += 1,
        }
    }

    fn view(&self) -> iced::Element<Self::Message> {
        container(
            column![MyWidget::new(MyMessage::MyWidgetPressed), text(self.count)]
                .spacing(20)
                .align_items(iced::Alignment::Center),
        )
        .width(Length::Fill)
        .height(Length::Fill)
        .center_x()
        .center_y()
        .into()
    }
}

struct MyWidget<Message> {
    pressed_message: Message,
}

impl<Message> MyWidget<Message> {
    fn new(pressed_message: Message) -> Self {
        Self { pressed_message }
    }
}

impl<Message, Renderer> Widget<Message, Theme, Renderer> for MyWidget<Message>
where
    Renderer: iced::advanced::Renderer,
    Message: Clone,
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

    fn on_event(
        &mut self,
        _state: &mut Tree,
        event: Event,
        layout: Layout<'_>,
        cursor: mouse::Cursor,
        _renderer: &Renderer,
        _clipboard: &mut dyn Clipboard,
        shell: &mut Shell<'_, Message>,
        _viewport: &Rectangle,
    ) -> event::Status {
        if cursor.is_over(layout.bounds()) {
            match event {
                Event::Mouse(mouse::Event::ButtonPressed(_)) => {
                    shell.publish(self.pressed_message.clone());
                    event::Status::Captured
                }
                _ => event::Status::Ignored,
            }
        } else {
            event::Status::Ignored
        }
    }
}

impl<'a, Message, Renderer> From<MyWidget<Message>> for Element<'a, Message, Theme, Renderer>
where
    Message: 'a + Clone,
    Renderer: iced::advanced::Renderer,
{
    fn from(widget: MyWidget<Message>) -> Self {
        Self::new(widget)
    }
}
```

![生成控件消息](./pic/producing_widget_messages.png)

:arrow_right: 下一步：[鼠标指针悬停在控件上](./mouse_pointer_over_widgets.md)

:blue_book: 返回：[目录](./../README.md)
