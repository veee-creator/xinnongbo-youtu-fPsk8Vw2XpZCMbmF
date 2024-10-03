
大家好，国庆节快乐，`Known`又更新了一波功能。


继上个版本以来，一直在完善网站文档、`KnownCMS`开源项目和解决框架客户提出的一些问题。这期间客户反馈的问题主要如下：


* 微信模板消息收不到，查其原因是网络问题，因此本版本改成后台任务发送消息
* 无代码页面模板需要自定义
* 无代码表单字段组件需要自定义
* 数据库访问需要支持`EFCore`
* 有些网友测试非常细致，发现了一些`BUG`


## 更新内容


* 🔨支持手动刷新微信访问`Token`
* 🔨优化启动程序错误日志
* 😄`Known.SqlSugar`从项目独立出来一个新库
* 🐛修复嵌入`iFrame`页面打不开问题
* 🐛修复系统许可密钥验证面板问题
* 🐛修复高级搜索字段显示问题
* 🐛修复数据字典无类别新增问题
* 🐛修复无代码模块日期查询条件控件问题
* 🐛修复异步导入文字不显示问题
* 😄微信模板消息发送增加后台任务记录
* 😄自动表格页面增加自定义配置选项
* 😄表格行支持双击事件
* 🔨优化表格查看和编辑多次弹窗问题
* 😄表单字段类型支持扩展的自定义组件
* 🔨优化`Database`组件，支持`EFCore`
* 🔨优化导入组件提示信息刷新问题
* 🔨`IDataRepository`改成依赖注入


## 详细内容


1. 增加了独立项目存储第三方数据库访问组件，项目地址如下



> [https://gitee.com/known/known\-db](https://github.com)


2. 自定义页面配置


* 在前端项目`AppClient.cs`中配置如下



```


|  | public static class AppClient |
| --- | --- |
|  | { |
|  | public static void AddSampleRazor(this IServiceCollection services) |
|  | { |
|  | // 配置建造自动表格页面方法委托 |
|  | UIConfig.AutoTablePage = (b, m) => b.Component().Set(c => c.Model, m).Build(); |
|  | } |
|  | } |


```

* 添加自定义自动表格页面组件`CustomTablePage.razor`页面



```


|  | @inherits BaseComponent |
| --- | --- |
|  |  |
|  | <div>@Model?.PageNamediv> |
|  |  |
|  | @code { |
|  | // 自动表格页面配置模型 |
|  | [Parameter] public TableModel> Model { get; set; } |
|  | } |


```

3. 自定义表单字段组件


* 当框架内置的表单字段组件`Input`、`Select`等不满足业务需求时，框架可以支持在项目中自定义
* 在【模块管理\-表单配置】中，字段类型选择`Custom`时，可以选择项目中自定义的字段组件
* 如何在这里能选择自己扩展的自定义字段组件，只要组件继承`ICustomField`接口即可，示例如下



```


|  | // CustomField是框架默认抽象自定义组件基类，可以换成项目中任何组件 |
| --- | --- |
|  | // ICustomField必须要继承，否则在线表单中不会显示 |
|  | class MyField : CustomField, ICustomField |
|  | { |
|  | protected override void BuildRender(RenderTreeBuilder builder) |
|  | { |
|  | // 在这里构建你的自定义组件内容，例如一个按钮 |
|  | builder.Button("自定义", this.Callback(OnClick)); |
|  | } |
|  |  |
|  | private void OnClick(MouseEventArgs args) { } |
|  | } |


```

* 效果如下
![image](https://img2024.cnblogs.com/blog/21278/202410/21278-20241002201707246-2067054318.gif)


4. `EFCore`配置如下


* 在`XXX.Web`项目中引用



```


|  | <Project Sdk="Microsoft.NET.Sdk.Web"> |
| --- | --- |
|  | <ItemGroup> |
|  | <PackageReference Include="Known.EntityFramework" Version="1.0.0" /> |
|  |  |
|  | <PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer" Version="8.0.8" /> |
|  | ItemGroup> |
|  | Project> |


```

* 在`Program.cs`文件中添加如下代码



```


|  | builder.Services.AddKnownEntityFramework(option => |
| --- | --- |
|  | { |
|  | // 配置数据库 |
|  | option.OnConfig = c => c.UseSqlServer(builder.Configuration.GetSection("ConnString").Get<string>()); |
|  | // 在此配置业务库数据模型 |
|  | //option.OnModel = m => m.Entity(); |
|  | }); |


```

 本博客参考[飞数机场](https://ze16.com)。转载请注明出处！
