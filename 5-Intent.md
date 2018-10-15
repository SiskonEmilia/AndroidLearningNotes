系统的信使 - Intent
===

在 [应用基础知识](1-应用基础知识.md#如何启动一个组件intent-那些事) 一章中，我们曾初步领略了 Intent 在 Android 系统中的重要地位，今天我们就来仔细研究一下这位系统的信使吧。

<!-- TOC -->

- [Intent 类型](#intent-类型)
- [构建 Intent](#构建-intent)

<!-- /TOC -->

## Intent 类型

- **显式 Intent：** 按完全限定类名制定要启动的组件。通常用于应用内的组件启动
- **隐式 Intent：** 不指定特定的组件，而是声明要执行的操作。系统将会列出所有符合条件的组件供用户选择使用。

**仅可使用显式 Intent 调用服务组件！**

## 构建 Intent
