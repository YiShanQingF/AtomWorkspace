### Sublime text 2/3 中 Package Control 的安装与使用方法

Package Control 插件是一个方便 Sublime text 管理插件的插件，但因为 Sublime Text 3 更新了 Python 的函数，API不同了，导致基于 Python 开发的插件很多都不能工作，Package Control 原来的安装方法都失效了。

网上有2种使用 Git 的安装方法，感觉太麻烦了。此处将 wbond 网站的 ST3 Package Control 简便安装方法翻译转至此处，方便大家查阅。

简单的安装方法：

从菜单 View - Show Console 或者 ctrl + ~ 快捷键，调出 console。将以下 Python 代码粘贴进去并 enter 执行，不出意外即完成安装。以下提供 ST3 和 ST2 的安装代码：

Sublime Text 3：
```
import urllib.request,os; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read())
```


[ 原文链接 ](http://www.imjeff.cn/blog/62/)
