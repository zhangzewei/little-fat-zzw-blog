# 制作生成icon-font步骤

1.  准备好图标的SVG文件
2.  进入[icomoon](https://icomoon.io/)
3.  在网页的右上角找到 `IcoMoon App` 的按钮，点击进入
4.  进入之后，将之前准备的Icon的SVG文件在左上角的 `Import Icons` 按钮上传
5.  上传之后在顶部菜单选项中点击 `笔` 样子的按钮，再点击上传的图标就能够对其进行修改;
6.  在修改的界面会有两个输入框一个是 `Tags` (用来在当前页面展示和搜索的)，另一个是 `Names` (用来命名你的这个图标的，也就是下载之后样式表之中真正使用的)
7.  完成所有图标的设置之后，页面下方有三个按钮，点击 `Generate Font` 
8.  这里就是展示刚才设置的图标信息，检查对比之后如果没有问题，就点击页面下方的刚才点击的那个菜单下方的 `Download` 侧边的一个齿轮按钮进行最后的设置
	+ `Font Name` 设置你喜欢的名称，这个会作为IconFont字体的名字 `zzwicon`
	+ `Class Prefix` 这个会成为字体类的前缀 `icon-`
	+ 多选框选择 `Support IE8` 和 `Generate preprocessor variables for: less`
	+ `CSS Selector` 单选框选择 `Use a class` 并在输入框中输入一个类名 `.zzwicon`
	+ 其余设置按照自己喜欢的来设置，关闭设置弹窗，点击 `Download` 下载
9.  将下载文件解压，使用其中的 `style.css` 文件，以及 `fonts` 文件夹
10.  在 `style.css` 文件中需要修改一些东西

	 ```css
		/* 我们需将下面的url路径改为引用的fonts文件夹的路径，根据实际情况修改 */
		@font-face {
		font-family: 'nbulicon';
		  src:  url('fonts/nbulicon.eot?q06q0o');
		  src:  url('fonts/nbulicon.eot?q06q0o#iefix') format('embedded-opentype'),
			  url('fonts/nbulicon.ttf?q06q0o') format('truetype'),
			  url('fonts/nbulicon.woff?q06q0o') format('woff'),
			  url('fonts/nbulicon.svg?q06q0o#nbulicon') format('svg');
		  ...
		}
		/* 下方这个类中的字体样式需要加上以下代码中的font-family */
		.zzwicon {
		  font-family: 'zzwicon',PingFangSC-Light,'helvetica neue','hiragino sans gb',arial,'microsoft yahei ui','microsoft yahei',simsun,sans-serif !important;
		  ...
		}
		.icon-msg:before {
 		  content: "\e900";
		}
	 ```

11.  然后就可以拿来用啦

```html
// 首先需要引入
<link href='path/to/style.css' />
// 然后可以在你需要用的地方使用上刚才的那些类名
<i class="icon zzwicon icon-msg"></i>
```
