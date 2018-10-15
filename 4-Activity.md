Android 世界之门 - Activity
===

<!-- TOC -->

- [从创建 Activity 说起](#从创建-activity-说起)
    - [实现用户界面](#实现用户界面)
    - [在清单文件中声明](#在清单文件中声明)
        - [Intent 过滤器](#intent-过滤器)
- [启动 Activity](#启动-activity)
    - [预期结果的启动](#预期结果的启动)
- [结束 Activity](#结束-activity)
- [管理 Activity 生命周期](#管理-activity-生命周期)
    - [生命周期回调](#生命周期回调)
    - [保存 Activity 状态](#保存-activity-状态)
    - [处理配置变更](#处理配置变更)

<!-- /TOC -->

作为用户直接接触应用程序的通道，**Activity** 提供一个可与用户交互的屏幕，它一般充满屏幕，但也可以作为小窗口浮动在其他窗口之上。

一个应用通常由多个联系松散的 Activity 组成，用户可以指定一个 **主 Activity**，来使得每次启动应用时，总会启动这个 Activity。每个 Activity 一经启动就存在于系统的 **返回栈** 中。当新的 Activity 被启动时，焦点将会被重定到新的 Activity 上，而旧的 Activity 仍会继续保留在返回栈中。在“返回”按钮被点击的时候，当前 Activity 会从栈中弹出（并且销毁），新的栈顶元素将会获得焦点。

## 从创建 Activity 说起

要创建一个 Activity，你需要实现一个 `Activity` 的子类，这意味着你必须实现其生命周期转变时的各种系统回调方法。有关这些方法的调用时机以及任务，您可以参照后文的 [管理 Activity 生命周期](#管理-activity-生命周期)。

### 实现用户界面

用户界面是一般在 `xml` 文件中定义，并由层级式视图（`View` 类的子类对象）提供的。每一个视图都控制着窗口内的一块矩形空间，并可根据设置对用户交互做出响应。

Android 提供了诸多部件和布局来帮助构建用户界面。您也可以通过 `View` 的子类和 `ViewGroup` 的子类分别创建自己的部件和布局。

`xml` 中定义的布局需要通过 `onCreate()` 方法中的 `setContentView()` 来和 Acitivity 绑定起来（这个方法也可以在其他地方使用）。当然，您也可以在代码中通过创建新的 `View` 或 `ViewGroup` 等实例来动态改变用户界面。

### 在清单文件中声明

因为 Activity 的管理是直接托管给 Android 系统的，所以您必须要在[清单文件](1-应用基础知识.md#组件清单)中声明，来让系统注册这个 Activity，才能正常访问他。有关内容还请点击 [清单文件](1-应用基础知识.md#组件清单)。

#### Intent 过滤器

关于 Intent 过滤器，我们还有些要提的内容。以下面的清单文件代码为例：

```xml
<activity android:name=".ExampleActivity" android:icon="@drawable/app_icon">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>
```

这部分内容在应用创建之初便被 IDE 自动生成，它们指定了 Activity 会响应 “主操作”（标识这是应用的主要入口点），并且具有“launcher”的类别（即会在系统的应用启动器中显示）。

## 启动 Activity

`startActivity()` 是用来启动一个 Activity 的不二之选。通过创建一个描述目标 Activity 的 Intent，并将其传递给该方法，我们就可以启动一个对应的 Activity。同时，Intent 还能够携带一些供 Activity 使用的数据。例如：

```kt
intent := new Intent(Intent.ACTION_SEND)
intent.putExtra(Intent.EXTRA_EMAIL, recipientArray)
startActivity(intent)
```

这段代码调用一个发送电子邮件的 Activity 并且将收件人的电子邮件地址（`EXTRA_EMAIL`）发送给它。

### 预期结果的启动

当你需要启动的应用为你返回一些数据的时候，`startActivityForResult()` 将会是你的最佳选择。使用该命令启动的应用将会在结束时将包含数据的 `Intent` 发送到您的 `onAcitivityResult()`  方法中。

一个例子是选取联系人：

```java
private void pickContact() {
    // Create an intent to "pick" a contact, as defined by the content provider URI
    Intent intent = new Intent(Intent.ACTION_PICK, Contacts.CONTENT_URI);
    startActivityForResult(intent, PICK_CONTACT_REQUEST);
}

@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    // If the request went well (OK) and the request was PICK_CONTACT_REQUEST
    if (resultCode == Activity.RESULT_OK && requestCode == PICK_CONTACT_REQUEST) {
        // Perform a query to the contact's content provider for the contact's name
        Cursor cursor = getContentResolver().query(data.getData(),
        new String[] {Contacts.DISPLAY_NAME}, null, null, null);
        if (cursor.moveToFirst()) { // True if the cursor is not empty
            int columnIndex = cursor.getColumnIndex(Contacts.DISPLAY_NAME);
            String name = cursor.getString(columnIndex);
            // Do something with the selected contact's name...
        }
    }
}
```

## 结束 Activity

显式地调用 Activity 的 `finish()` 方法可以结束该 Activity。如果您要结束您启动的另一个 Activity，`finishActivity()` 可以帮到您。但是使用这两个方法不是一种很好的实践。这会破坏用户预期的 **Activity 生命周期**。

## 管理 Activity 生命周期

Activity 以三种状态存在：

- 继续（运行中）

    Activity 位于屏幕前台并具有用户焦点。

- 暂停

    另一 Activity 位于屏幕前台，但是该 Activity 仍然可见。此时的 Activity 仍旧处于完全活动状态。可能被释放以提供可用内存。

- 停止

    被其它 Activity 完全遮蔽。处于活动状态，但是不与窗口管理器保持连接。可能被释放以提供可用内存。

### 生命周期回调

[activity_lifecycle](Assets/activity_lifecycle.png)

需要注意的是，当由于内存不足而导致应用被紧急释放时，`onStop()` 和 `onDestroy()` 或许不会被调用，因而在 `onPause()` 方法中，您就应该做好关键数据的持久性处理。

### 保存 Activity 状态

[restore_instance](Assets/restore_instance.png)

系统会先调用 `onSaveInstanceState()`，然后再使 Activity 变得易于销毁。系统会向该方法传递一个 `Bundle`，您可以在其中使用 `putString()` 和 `putInt()` 等方法以名称-值对形式保存有关 Activity 状态的信息。然后，如果系统终止您的应用进程，并且用户返回您的 Activity，则系统会重建该 Activity，并将 `Bundle` 同时传递给 `onCreate()` 和 `onRestoreInstanceState()`。您可以使用上述任一方法从 Bundle 提取您保存的状态并恢复该 Activity 状态。如果没有状态信息需要恢复，则传递给您的 `Bundle` 是空值（如果是首次创建该 Activity，就会出现这种情况）。

无法保证系统会在销毁您的 Activity 前调用 `onSaveInstanceState()`，因为存在不需要保存状态的情况（例如用户使用“返回”按钮离开您的 Activity 时，因为用户的行为是在显式关闭 Activity）。

### 处理配置变更

有些设备配置可能会在运行时发生变化（例如屏幕方向、键盘可用性及语言）。 发生此类变化时，Android 会重建运行中的 Activity（系统调用 `onDestroy()`，然后立即调用 `onCreate()`）。处理此类重启的最佳方法是利用 `onSaveInstanceState()` 和 `onRestoreInstanceState()`（或 `onCreate()`）保存并恢复 Activity 的状态。