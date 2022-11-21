在inoreader中阅读文章时，文章的封面无法载入，而其中的内容也无法完全载入。

只有根据标题，选择自己感兴趣的文章，然后去阅读全文，实在是有点不方便。

于是有了给inoreader增加一个阅读界面的想法。

好在我不是第一个觉得inoreader在这方面做的不好的人。

市面上有在这方面有提升的竞品，如feeder.co。

但是这些竞品的限制较高，免费版本难以满足我的需求。

而在改造inoreader方面，有许多油猴脚本能够提升inoreader。如 [https://greasyfork.org/zh-CN/scripts/897-inoreader-full-feed](https://greasyfork.org/zh-CN/scripts/897-inoreader-full-feed)

inoreader-full-feed脚本质量确实不错，功能强大，但是却并不完全是我想要的，我的需求比它的功能要更简单。

我想直接插入iframe，这样其他的油猴脚本都能够正常工作。

于是我在它的基础上进行改造，直接进行简单粗暴的修改，修改它的相关按键响应，直接将iframe插入，满足我现在的需求。

在这个改造上，我的思路是**不重复造轮子**，我的目标是满足我现有的需求，即 **点到即止** 。