# 设置环境

初始化一个 [Cargo](https://doc.rust-lang.org/cargo/guide/) 项目。

```sh
cargo new my_project
```

这里的 `my_project` 是你的项目名称。

将 [Iced](https://iced.rs/) 添加到项目的依赖中。

```sh
cd my_project
cargo add iced
```

你应该能在 `Cargo.toml` 文件的末尾看到依赖项。

```toml
[dependencies]
iced = "0.12.1"
```

注意：如果你遇到了 `WGPU Error`，你可以在 `Cargo.toml` 文件中禁用 `wgpu`。

```toml
[dependencies]
iced = { version = "0.12.1", default-features = false }
```

:arrow_right: 下一步：[第一个应用 - Hello World!](./first_app.md)

:blue_book: 返回：[目录](./../README.md)
