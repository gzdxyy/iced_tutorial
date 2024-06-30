
# 异步加载图像

在前面的教程中，我们介绍了如何使用 [Application](https://docs.rs/iced/0.12.1/iced/application/trait.Application.html) 执行异步操作以及如何显示图像。
本教程结合了这两者，演示了如何异步加载图像。

假设在 Cargo 项目根目录中有一个名为 `ferris.png` 的图像。

为了使用异步和图像功能，我们必须包含相应的异步库并启用相应的特性。
`Cargo.toml` 文件的依赖项应该如下所示：

```toml
[dependencies]
iced = { version = "0.12.1", features = ["image", "tokio"] }
tokio = { version = "1.36.0", features = ["full"] }
```

我们的应用程序将有三个状态：*开始*、*加载中* 和 *已加载*。
我们使用两个字段来编码这三个状态。

```rust
struct MyApp {
    image_handle: Option<Handle>,
    show_container: bool,
}
```

当状态是

* **开始**：`image_handle` 是 `None`，`show_container` 是 false；
* **加载中**：`image_handle` 是 `None`，`show_container` 是 true；
* **已加载**：`image_handle` 是 `Some(...)`，`show_container` 是 true。

应用程序从 *开始* 状态开始。

应用程序总是显示一个用于加载 `ferris.png` 图像的按钮。
在 *开始* 状态，应用程序不显示任何额外的控件。
在 *加载中* 状态，应用程序显示文本 `Loading...`。
在 *已加载* 状态，应用程序显示图像。

```rust
fn view(&self) -> iced::Element<Self::Message> {
    column![
        button("Load").on_press(MyMessage::Load),
        if self.show_container {
            match &self.image_handle {
                Some(h) => container(image(h.clone())),
                None => container("Loading..."),
            }
        } else {
            container("")
        },
    ]
    .padding(20)
    .into()
}
```

我们为应用程序有两个消息：

```rust
#[derive(Debug, Clone)]
enum MyMessage {
    Load,
    Loaded(Vec<u8>),
}
```

当按钮被按下时，应用程序触发一个 `Load` 消息来加载图像。
当图像加载时，应用程序触发一个 `Loaded(...)` 消息。

图像将被异步加载。

```rust
fn update(&mut self, message: Self::Message) -> iced::Command<Self::Message> {
    match message {
        MyMessage::Load => {
            self.show_container = true;
            return Command::perform(
                async {
                    let mut file = File::open("ferris.png").await.unwrap();
                    let mut buffer = Vec::new();
                    file.read_to_end(&mut buffer).await.unwrap();
                    buffer
                },
                MyMessage::Loaded,
            );
        }
        MyMessage::Loaded(data) => self.image_handle = Some(Handle::from_memory(data)),
    }
    Command::none()
}
```

完整代码如下：

```rust
use iced::{
    executor,
    widget::{button, column, container, image, image::Handle},
    Application, Command,
};
use tokio::{fs::File, io::AsyncReadExt};

fn main() -> iced::Result {
    MyApp::run(iced::Settings::default())
}

#[derive(Debug, Clone)]
enum MyMessage {
    Load,
    Loaded(Vec<u8>),
}

struct MyApp {
    image_handle: Option<Handle>,
    show_container: bool,
}

impl Application for MyApp {
    type Executor = executor::Default;
    type Message = MyMessage;
    type Theme = iced::Theme;
    type Flags = ();

    fn new(_flags: Self::Flags) -> (Self, iced::Command<Self::Message>) {
        (
            Self {
                image_handle: None,
                show_container: false,
            },
            Command::none(),
        )
    }

    fn title(&self) -> String {
        String::from("My App")
    }

    fn update(&mut self, message: Self::Message) -> iced::Command<Self::Message> {
        match message {
            MyMessage::Load => {
                self.show_container = true;
                return Command::perform(
                    async {
                        let mut file = File::open("ferris.png").await.unwrap();
                        let mut buffer = Vec::new();
                        file.read_to_end(&mut buffer).await.unwrap();
                        buffer
                    },
                    MyMessage::Loaded,
                );
            }
            MyMessage::Loaded(data) => self.image_handle = Some(Handle::from_memory(data)),
        }
        Command::none()
    }

    fn view(&self) -> iced::Element<Self::Message> {
        column![
            button("Load").on_press(MyMessage::Load),
            if self.show_container {
                match &self.image_handle {
                    Some(h) => container(image(h.clone())),
                    None => container("Loading..."),
                }
            } else {
                container("")
            },
        ]
        .padding(20)
        .into()
    }
}
```

*开始* 状态：

![异步加载图像 1](./pic/loading_images_asynchronously_1.png)

*加载中* 状态：

![异步加载图像 2](./pic/loading_images_asynchronously_2.png)

*已加载* 状态：

![异步加载图像 3](./pic/loading_images_asynchronously_3.png)

:blue_book: 返回：[目录](./../README.md)
