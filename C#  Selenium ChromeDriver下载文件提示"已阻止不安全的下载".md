## Selenium ChromeDriver下载文件提示"以阻止不安全的下载"
Chrome浏览器下载文件会提示"已阻止不安全的下载"，需要手动点击保留，用Selenium做自动化下载的话就卡住了，后面在[stackoverflow](https://stackoverflow.com/questions/78057740/chrome-122-how-to-allow-insecure-content-insecure-download-blocked "stackoverflow")找到了解决方案。

options.AddArgument("--unsafely-treat-insecure-origin-as-secure=http://xxx.com") ;替换自己的域名,多域名可用逗号分隔。

完整代码如下：
```csharp
ChromeOptions options = new ChromeOptions(); 
options.AddExcludedArgument("enable-automation");   //去除状态栏【正在受到自动软件的控制】的提示 
options.AddArgument("--unsafely-treat-insecure-origin-as-secure=http://xxx.com");
var downloadDirectory = "yourpath";
options.AddUserProfilePreference("download.default_directory", downloadDirectory);  
//禁止弹出密码保存对话框
options.AddUserProfilePreference("credentials_enable_service", false); 
using (IWebDriver driver = new ChromeDriver("c:\\xxx\\chrome-win64", options))
{
  driver.Navigate().GoToUrl("http://xxx.com/login");
  driver.Quit();
}
```
