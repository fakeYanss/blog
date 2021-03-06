---
title: 倾斜人生 - 1
date: 2019-05-15 22:15:05
tags: tip
---
201905W20 Tip

<!--more-->

---
## Hexo tag shortcut in hollow

验证 Hexo 的内置标签在 hollow 中可使用的部分。

### Block Quote

```
{% blockquote David Levithan, Wide Awake %}
Do not just seek happiness for yourself. Seek happiness for all. Through kindness. Through mercy.
{% endblockquote %}
```

{% blockquote David Levithan, Wide Awake %}
Do not just seek happiness for yourself. Seek happiness for all. Through kindness. Through mercy.
{% endblockquote %}

### Code Block
```
{% codeblock _.compact http://underscorejs.org/#compact Underscore.js %}
_.compact([0, 1, false, 2, '', 3]);
=> [1, 2, 3]
{% endcodeblock %}
```
{% codeblock _.compact http://underscorejs.org/#compact Underscore.js %}
_.compact([0, 1, false, 2, '', 3]);
=> [1, 2, 3]
{% endcodeblock %}

### YouTube
```
{% youtube video_id %}
```


___


## Mysql 命令
查询数据库编码方式：
```sql
show variables like 'character%';
```

将查询语句结果输出到文件
```sh
mysql -h127.0.0.1 -P3306 -uroot -Ddatabases -p -e "select * from table" > file
```

___

## 将 bash 换成 zsh & oh my zsh
一旦你使用了 zsh 和 oh my zsh，你就很难换回 bash 了。

如何在服务器上使用 zsh 和 oh my zsh？

**第一步：**

以 Ubuntu 为例，首先安装 zsh。
```sh
sudo apt install zsh
```

然后将默认 Shell 设置为 zsh，有可能路径是 `/bin/zsh`
```sh
 chsh -s /usr/bin/zsh
```

检查。
```sh
echo $SHELL
```

第二步：

安装 oh my zsh。
```sh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

第三步：

配置插件，命令提示和高亮
```sh
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

修改 `vim .zshrc`，找到 `plugins=(git)` 改为 `plugins=(git zsh-autosuggestions zsh-syntax-highlighting)`

并修改主题为 `ZSH_THEME="agnoster"`。

然后退出保存。

这个主题的路径前缀太长，编辑`~/.oh-my-zsh/themes/agnoster.zsh-theme`，将 `build_prompt` 下的 `prompt_context` 注释掉。

最后
```sh
source .zshrc
```

如果这时候发现显示有些乱码，可以换一下字体，如果没有可用的字体，可以安装powerline的字体。
```
git clone https://github.com/powerline/fonts.git
cd fonts
./install.sh
```

重连一下服务器，打开新世界。

___

## 印象笔记 Markdown 支持什么快捷键？

**Mac 端**

| Command            | Shortcut    |
| ------------------ | ----------- |
| 新建 Markdown 笔记 | CMD+D       |
| 粗体               | CMD+B       |
| 斜体               | CMD+I       |
| 删除线             | CMD+S       |
| 分隔线             | CMD+L       |
| 编号列表           | CMD+Shift+O |
| 项目符号列表       | CMD+Shift+U |
| 插入待办事项       | CMD+Shift+T |
| 代码块             | CMD+Shift+P |
| 撤销               | CMD+Z       |
| 在笔记内搜索       | CMD+F       |

**Windows 端**

| Command            | Shortcut         |
| ------------------ | ---------------- |
| 新建 Markdown 笔记 | Ctrl+Alt+D       |
| 粗体               | Ctrl+B           |
| 斜体               | Ctrl+I           |
| 删除线             | Ctrl+T           |
| 下划线             | Ctrl+U           |
| 分隔线             | Ctrl + Shift + - |
| 编号列表           | Ctrl + Shift + O |
| 项目符号列表       | Ctrl + Shift + W |
| 插入待办事项       | Ctrl + Shift + C |
| 代码块             | Ctrl+Shift+L     |
| 插入日期和时间     | Alt + Shift + D  |
| 撤销               | Ctrl+Z           |
| 在笔记内搜索       | Ctrl+F           |


