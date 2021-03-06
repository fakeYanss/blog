---
title: TODO with Mac Menubar
date: 2019-05-17 20:18:43
tags: Tool
---
BitBar 使我工作更快乐！

<!-- more -->

---

最近觉得每天的计划总是完不成，明明也不多，但是很容易被其他事情干扰。我一直不喜欢用日程表来排每天的事情，但我很喜欢给自己制定一个 TODO 列表，这个列表包含了短期内要做的事情，但不限于某一天或者某一个小时，不至于太细致，让自己经常不能按时完成，获得过多的挫败感。

我希望的展现形式是在电脑桌面上时刻可以看到的，这样的话，就不能用各种流行的 GTD 工具。像 OmniFocus、Microsoft To-Do等，都需要先打开再确认的过程。

像桌面便签也不太好，因为平时是打开了不同的应用铺满了桌面。

于是想到了菜单栏上可以做点事情。

找了一圈，Mac平台上的 TODO in menubar 的应用还不少，最合心意的是 [TODO Menubar](https://www.mactodo.app/?ref=producthunt)。
![TODO Menubar](https://foreti.me/imgplace/2019/20190517153927.png)
但是它收费...

还有一个[Doo](https://itunes.apple.com/us/app/doo-get-things-done/id1066322956?mt=8)，也收费，而且太花哨了，好看但不直观。
![](https://foreti.me/imgplace/2019/20190517160402.png)


在 Github 上也搜到了一个[postit-todo](https://github.com/Praseetha-KR/postit-todo)，不错但是用起来太卡，而且关闭后重新打开加载数据很慢。
![](https://foreti.me/imgplace/2019/20190517160158.png)

最后，在 AppSo 的一篇推 TODO Menubar 的文章评论里看到了 [BitBar](https://getbitbar.com/)。

BitBar 是一个可以编写脚本，将你能想到的任何信息，放到 menubar 里显示。

我直接在 homebrew 下载了 BitBar，`brew cask install bitbar`，并且在它的插件市场里找了一个 TODO 的[脚本](https://raw.githubusercontent.com/matryer/bitbar-plugins/master/Tools/todolist.2m.sh)。这个脚本实现的是在 Mac 的原生 Reminder 应用里指定一个提醒列表，将这个列表里的提醒事项放到菜单栏显示，并且只显示第一个，在菜单栏点击这个提醒事项，可以将它在 Reminder 里标记为完成。觉得这个脚本功能有点弱，还差点我要的东西。

顺着这个脚本的思路，边查 AppleScript 的语法边修改，最后改成了我要的样子，和 TODO Menubar 有点像。菜单栏显示完成情况，子菜单显示具体的 TODO，点击 TODO 可以标记为完成。

创建脚本文件`todo.2m.sh`，2m表示这个脚本2m执行一次，这是bitbar提供的功能；

脚本内容如下：

```shell
#!/bin/bash

TODO="Todo" # your todo list name in reminder
ARCHIVE="Archive" # your archive list name in reminder

reminder=$(osascript -e "tell application \"Reminders\"
	set activeReminders to (reminders of list \"$TODO\" whose completed is true)
	set numOfActiveReminders to (count of activeReminders)
	set allReminders to (reminders of list \"$TODO\")
	set numOfAllReminders to (count of allReminders)
	set result to ((numOfActiveReminders as string) & \"/\" & numOfAllReminders as string)
	return result
end tell")

todolist=$(osascript -e "tell application \"Reminders\"
	set todos to (reminders of list \"$TODO\" whose completed is false)
	set newlist to {}
	repeat with todo in todos
		copy (name of todo as text) to the end of the newlist
	end repeat
	return newlist
end tell")

todolist=${todolist// /#holder} # 删除空格
todolist=${todolist//,/ } # 转换 AppleScript 的 list 为 shell 的 list 格式

if [ "$1" = "done" ]; then
	osascript -e 	"tell application \"Reminders\"
	set activeReminders to (reminders of list \"$TODO\" whose completed is false)
	repeat with todo in activeReminders
		if (name of todo as text) is equal to \"$2\" then
			tell todo
				set completed to true
			end tell
		end if
	end repeat
end tell"
fi

if [ "$1" = "open" ]; then
	osascript -e 	'tell application "Reminders" to activate'
fi

if [ "$1" = "archive" ]; then
	osascript -e 	"tell application \"Reminders\"
    set output to \"\"
    set newList to list \"$ARCHIVE\"
    show newList
    repeat with thisReminder in (get reminders in list \"$TODO\" whose completed is true)
        set nameObj to name of thisReminder
        move thisReminder to newList
    end repeat
    return output
end tell"
fi


echo "✔︎ $reminder"
echo "---"
echo "⚙︎ Open Reminder| bash='$0' param1='open' terminal=false"
echo "---"
for loop in $todolist
do
	echo "${loop//#holder/ }| bash='$0' param1='done' param2='${loop//#holder/ }' terminal=false refresh=true"
done | sort
echo "---"
echo "↻ Refresh| terminal=false refresh=true"
echo "---"
echo "♺ Archive| bash='$0' param1='archive' terminal=false refresh=true"

```


> 2020-09-27 update: 由于手动移动 todo 到 Archive 比较麻烦，所以增加了 archive 功能。
> 2020-09-28 update: 由于去掉 todo 的空格，会导致点击该 todo 无法在 reminder 中自动勾选完成，所以在脚本内部做了优化，从 reminder 中读取列表时，将空格转为一个占位符，然后在 menubar 中展示时，将占位符还原为空格。


效果图：

![](https://foreti.me/imgplace/2019/20190523204949.png)

对应在 Reminder 中的任务列表

![](https://foreti.me/imgplace/2019/20190523150457.png)



(已优化)~~如果你细心点，可以发现第 3 条 todo，`提交 commit` 在菜单栏中变成了 `提交commit`，这是由于 shell 中的数组元素以空格区分，为了避免1 个 todo 显示在两行，我将每个 todo 中的空格都去掉了。~~

~~这样，每天只用维护自己的 Reminder 的任务列表就行了。如果很多天过去，任务总数太多，可以再创建一个 Archive 提醒列表，将过去完成的任务都移动到里面。~~

快乐的写 TODO 吧！

