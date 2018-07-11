# 无埋点采集事件

## 访问数据

每次用户打开小程序的时候，发送一条`应用打开`消息，对应的数据指标是打开次数。发送数据包含但不限于以下信息：进入页面、进入时间、场景值、来源小程序或 App、设备信息、微信信息、应用版本号。

每次用户关闭小程序的时候，关闭动作包括退出小程序回到微信、退出微信，会发送一条`应用关闭`消息，会根据这个消息来计算应用访问时长。发送数据包含但不限于以下信息：退出页面、退出时间。

## 页面数据

每次用户访问一个新的页面，发送一条`页面打开`消息，对应的数据意义是页面分析。发送数据包含但不限于以下信息：页面信息、打开时间、页面来源、页面标题、页面级变量。

每次当用户点击转发按钮时，会弹出转发框，这时会发送一条`要转发消息`消息。这是一个自定义事件，数据包含以下信息：事件时间、所在页面、转发动作来源、转发页面标题和转发页面路径。注意这个事件不代表用户真正转发了消息到聊天里面，而是用户触发了转发动作。如果要跟踪成功转发消息事件，建议在 onShareAppMessage 的 success callback 里面发送一个自定义事件。

## 行为数据

对于用户在页面发生的行为，如果这个行为有绑定事件比如 bindtap，并且在你的小程序里面进行处理，那么 GrowingIO 的小程序 SDK 会自动采集这些事件，发送一条行为消息。目前，我们支持的事件有 tap、longpress、confirm 和 change 事件。

### tap

tap 事件是手指触摸后马上离开时触发的事件。当 wxml 中的 view 绑定了 bindtap 事件以后，在事件处理函数执行的时候，SDK 会自动采集 tap 事件，发送数据包含但不限于以下信息：点击事件时间、事件发生所在页面、点击控件相关信息。比如如下，

```text
<view data-title='复仇者联盟3' data-index='1' bindtap='clickMovie'>  <image src='IMAGE—URL' mode='aspectFill'/>  <text>复仇者联盟3</text></view>
```

注意这里的 `data-title` 和 `data-index` 属性，因为微信小程序的限制，无法采集到控件的内容和结构数据，所以在小程序 SDK 里面我们采取的是声明式编程，通过在 wxml 文件里面设置 data- 属性，可以给 view 控件添加额外的`内容`和`位置`属性，方便后续在分析时可以按照元素内容和元素位置做分析，对于列表式的组件特别有用和方便。

**建议在 View 的控件里面多使用 data-title 和 data-index 属性这种声明式编程方式。**

### longpress

longpress 事件是手指触摸后，超过350ms再离开时触发的事件。当 wxml 的 view 绑定了 bindlongpress 事件以后，在事件处理函数执行的时候，SDK 会自动采集 longpress 事件，发送数据包含但不限于以下信息：点击事件时间、事件发生所在页面、点击控件相关信息。比如如下，

```text
<view data-title='复仇者联盟3' data-index='1' bindtap='clickMovie'>
  <image src='IMAGE—URL' mode='aspectFill'/>
  <text>复仇者联盟3</text>
</view>
```

注意这里的 `data-title` 和 `data-index` 属性，因为微信小程序的限制，无法采集到控件的内容和结构数据，所以在小程序 SDK 里面我们采取的是声明式编程，通过在 wxml 文件里面设置 data- 属性，可以给 view 控件添加额外的`内容`和`位置`属性，方便后续在分析时可以按照元素内容和元素位置做分析，对于列表式的组件特别有用和方便。

**建议在 View 的控件里面多使用 data-title 和 data-index 属性这种声明式编程方式。**

### change

change 事件是针对 checkbox, radio, picker-view 这些控件，当选择项发生改变时触发的事件。当 wxml 的 view 绑定了 bingchange 事件以后，在事件处理函数执行的时候，SDK 会自动采集 change 事件，发送数据包含但不限于以下信息：选择事件的发生时间、事件发生所在页面。如果设置了要采集内容，则也会包含选择项的内容信息。比如如下，

```text
<checkbox-group bindchange='checkboxChange' data-growing-track>
  <label class='checkbox'>
    <checkbox value='GrowingIO' checked='true' /> GrowingIO
  </label>
  <label class='checkbox'>
    <checkbox value='Tencent' checked='false' /> 腾讯小程序分析工具
  </label>
  <label class='checkbox'>
    <checkbox value='Google' checked='false' /> Google Analytics
  </label>
</checkbox-group>
```

当用户选择了某项以后，因为这里设置了 `data-growing-track` 属性，所以会采集到这一项对应的 value 值。

### confirm

confirm 事件是对于 input 和 textarea 控件，当输入完成后触发的事件。当 wxml 的 view 绑定了 bindconfirm 事件以后，在事件处理函数执行的时候，SDK 会自动采集 confirm 事件，发送数据包含但不限于以下信息：输入事件的发生时间、事件发生所在页面。如果设置了要采集内容，则也会包含输入的内容。比如如下，

```text
<input class='new-todo'
       value='{{ input }}'
       placeholder='Anything here...'
       data-growing-track='true'
       bindinput='inputChangeHandle'
       bindconfirm='addTodoHandle'
/>
```

当用户输入完成后，因为这里指定了 `data-growing-track` 属性，所以会采集到输入的内容。

