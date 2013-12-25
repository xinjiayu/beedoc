---
name: URL构建
sort: 2
---

## URL 构建
如果可以匹配 URL ，那么 beego 也可以生成 URL 吗？当然可以。 UrlFor() 函数就是用于构建指定函数的 URL 的。它把对应控制器和函数名结合的字符串作为第一个参数，其余参数对应 URL 中的变量。未知变量将添加到 URL 中作为查询参数。 例如：

下面定义了一个相应的控制器

```
type TestController struct {
	Controller
}

func (this *TestController) Get() {
	this.Data["Username"] = "astaxie"
	this.Ctx.Output.Body([]byte("ok"))
}

func (this *TestController) List() {
	this.Ctx.Output.Body([]byte("i am list"))
}

func (this *TestController) Params() {
	this.Ctx.Output.Body([]byte(this.Ctx.Input.Params["0"] + this.Ctx.Input.Params["1"] + this.Ctx.Input.Params["2"]))
}

func (this *TestController) Myext() {
	this.Ctx.Output.Body([]byte(this.Ctx.Input.Param(":ext")))
}

func (this *TestController) GetUrl() {
	this.Ctx.Output.Body([]byte(this.UrlFor(".Myext")))
}
```

下面是我们注册的路由：

```
beego.Router("/api/list", &TestController{}, "*:List")
beego.Router("/person/:last/:first", &TestController{})
beego.AutoRouter(&TestController{})
```

那么通过方式可以获取相应的URL地址：

```
UrlFor("TestController.List")
// 输出 /api/list

UrlFor("TestController.Get", ":last", "xie", ":first", "asta")
// 输出 /person/xie/asta

UrlFor("TestController.Myext")
// 输出 /Test/Myext

UrlFor("TestController.GetUrl")
// 输出 /Test/GetUrl
```

## 模板中如何使用
默认情况下，beego已经注册了urlfor函数，用户可以通过如下的代码进行调用

	{{urlfor "TestController.List"}}