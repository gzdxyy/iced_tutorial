# Iced 库非官方教程

[Iced](https://iced.rs/) 是一个用于 [Rust](https://www.rust-lang.org/) 的跨平台图形用户界面(GUI)库。
本教程旨在为该库提供一个快速入门指南。
我们尽量保持教程的每个部分尽可能简单。

本教程正在从 Iced 版本 `0.10.0` 更新至 `0.12.x`。
对于 `0.10.0` 版本的完整教程，请参考 [此分支](https://github.com/fogarecious/iced_tutorial/tree/0.10.x)。

内容：

* [设置环境](./tutorial/setting_up.md)
* [第一个应用 - Hello World!](./tutorial/first_app.md)
* [Sandbox Trait 说明](./tutorial/explanation_of_sandbox_trait.md)
* [添加控件](./tutorial/adding_widgets.md)
* [更改显示内容](./tutorial/changing_displaying_content.md)
* 控件
  * [文本](./tutorial/text.md)
  * [按钮](./tutorial/button.md)
  * [文本输入](./tutorial/text_input.md)
  * [复选框](./tutorial/checkbox.md)
  * [切换器](./tutorial/toggler.md)
  * [单选按钮](./tutorial/radio.md)
  * [选择列表](./tutorial/picklist.md)
  * [下拉框](./tutorial/combobox.md)
  * [滑块和垂直滑块](./tutorial/slider.md)
  * [进度条](./tutorial/progressbar.md)
  * [工具提示](./tutorial/tooltip.md)
  * [分隔线](./tutorial/rule.md)
  * [图像](./tutorial/image.md)
  * [SVG](./tutorial/svg.md)
  <!-- * 面板网格 -->
* 布局
  * [宽度和高度](./tutorial/width_and_height.md)
  * [列](./tutorial/column.md)
  * [行](./tutorial/row.md)
  * [空白](./tutorial/space.md)
  * [容器](./tutorial/container.md)
  * [可滚动](./tutorial/scrollable.md)
  <!-- * 响应式 -->
* [更改主题](./tutorial/changing_themes.md)
* 样式
  * [更改样式](./tutorial/changing_styles.md)
  * [自定义样式](./tutorial/custom_styles.md)
* 多页应用
  * [超过一个页面](./tutorial/more_than_one_page.md)
  * [无记忆页面](./tutorial/memoryless_pages.md)
  * [跨页面传递参数](./tutorial/passing_parameters_across_pages.md)
  * [导航历史](./tutorial/navigation_history.md)
* 应用
  * [从 Sandbox 到应用](./tutorial/from_sandbox_to_application.md)
  * [通过命令控制控件](./tutorial/controlling_widgets_by_commands.md)
  * [批量命令](./tutorial/batch_commands.md)
  * [执行自定义命令](./tutorial/executing_custom_commands.md)
* 窗口
  * [初始化不同的窗口](./tutorial/initializing_a_different_window.md)
  * [动态更改窗口](./tutorial/changing_the_window_dynamically.md)
  * [按需关闭窗口](./tutorial/closing_the_window_on_demand.md)
* 事件
  * [一些控件的按下/释放](./tutorial/on_pressed_released_of_some_widgets.md)
  * [鼠标事件生成消息](./tutorial/producing_messages_by_mouse_events.md)
  * [键盘事件生成消息](./tutorial/producing_messages_by_keyboard_events.md)
  * [定时器生成消息](./tutorial/producing_messages_by_timers.md)
  * [批量订阅](./tutorial/batch_subscriptions.md)
* 画布
  * [绘制形状](./tutorial/drawing_shapes.md)
  * [使用缓存绘制](./tutorial/drawing_with_caches.md)
* 自定义控件
  * [绘制控件](./tutorial/drawing_widgets.md)
  * [从外部更新控件](./tutorial/updating_widgets_from_outside.md)
  * [从事件更新控件](./tutorial/updating_widgets_from_events.md)
  * [生成控件消息](./tutorial/producing_widget_messages.md)
  * [鼠标指针悬停在控件上](./tutorial/mouse_pointer_over_widgets.md)
  * [控件中的文本](./tutorial/texts_in_widgets.md)
  * [自定义背景](./tutorial/custom_background.md)
  * [带有子控件的控件](./tutorial/widgets_with_children.md)
  * [获取任何子控件](./tutorial/taking_any_children.md)
* 其他
  * [异步加载图像](./tutorial/loading_images_asynchronously.md)

<!-- examples/component -->

## 参考

* [Iced](https://github.com/iced-rs/iced)  - Iced 库。
* [awesome-iced](https://github.com/iced-rs/awesome-iced)  - 依赖 Iced 的项目列表。

## 贡献

欢迎提交拉取请求以修正错别字、错误信息或其他想法！

## 许可

教程中的所有代码均在 MIT 许可下提供。
