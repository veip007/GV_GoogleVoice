## Google Voice 保号/自动发送及回复信息

### 一、自动发送信息

1、注册登录 [IFTTT](https://www.moeelf.com/go/aHR0cHM6Ly9pZnR0dC5jb20=)

2、配置 [Keep Google Voice Active (Send Messege)](https://www.moeelf.com/go/aHR0cHM6Ly9pZnR0dC5jb20vYXBwbGV0cy9Tc254VFlaSi1rZWVwLWdvb2dsZS12b2ljZS1hY3RpdmUtc2VuZC1tZXNzZWdl) (时区注意选择BeiJing。可以自己定义发送的时间及发送信息的内容。)

3、设置好后即可自动给你的 GV 码发送信息了。（你可以设置一下离你现在时间最近的时间测试。功能是已经测试过的没有问题的。）


### 二、自动回复信息

1、登入 GV，先在 GV 里面设置里面把“将消息转发到电子邮件”打开。

2、登入 Gmail，在设置里选择“过滤器和屏蔽的地址” --> “创建新的过滤器” --> 在发件人处填写 “@txt.voice.google.com”。如下图所示：

![1](https://camo.githubusercontent.com/f52a4f300edb7c383bb829216605a8c18b411801/68747470733a2f2f7777772e6d6f65656c662e636f6d2f616374696f6e2f57617465726d61726b3f6d61726b3d4c33567a63693931634778765957527a4c7a49774d546b764d4455764f444d344d7a55304f444d794c6e42755a773d3d)

3、点击“创建过滤器”，在弹出的对话框点击“选择标签” --> “新建标签”，输入标签名为“autoreply”，点击创建即可。

![2](https://camo.githubusercontent.com/9aaf099afeaee93354aaaa045a19b920f63b8608/68747470733a2f2f7777772e6d6f65656c662e636f6d2f616374696f6e2f57617465726d61726b3f6d61726b3d4c33567a63693931634778765957527a4c7a49774d546b764d4455764d5441344d5467354d4449344f533577626d633d)
4、选择如下图所示后点击“创建过滤器”即可。

![3](https://camo.githubusercontent.com/9aaf099afeaee93354aaaa045a19b920f63b8608/68747470733a2f2f7777772e6d6f65656c662e636f6d2f616374696f6e2f57617465726d61726b3f6d61726b3d4c33567a63693931634778765957527a4c7a49774d546b764d4455764d5441344d5467354d4449344f533577626d633d)


5、登录 Google Drive，单击左上角的“新建”。按下图新建一个 Google App Script。(如未找到可以在“关联更多应用”里面查找“Google Apps Script”关联一下就有了。)

![4](https://camo.githubusercontent.com/6992412e3846375845b524f4dcb4b81572597b83/68747470733a2f2f7777772e6d6f65656c662e636f6d2f616374696f6e2f57617465726d61726b3f6d61726b3d4c33567a63693931634778765957527a4c7a49774d546b764d4455764d7a59774f444d314d444d7a4d793577626d633d)

6、复制下面的代码替换现有的代码。
```bash
function autoReplier() {
  var labelObj = GmailApp.getUserLabelByName('autoreply');
  var gmailThreads;
  var messages;
  var messagecount;
  var sender;
  var num = 9;  //设置连续自动回复邮件的次数（为防止两人都是自动回复，当发送次数达到 9 时将不自动回复）。
  var hours = 12;  //如果自动回复次数超过了上面设置的值，过了多少小时后又可以自动回复。
    
  for (var gg = 0; gg < labelObj.getUnreadCount(); gg++) {
    gmailThreads = labelObj.getThreads()[gg];
    messages = gmailThreads.getMessages();
    messagecount = gmailThreads.getMessageCount();
    for (var ii = 0; ii < messages.length; ii++) {
      
      if (messages[ii].isUnread()) {
        
        msg = messages[ii].getPlainBody();
        sender = messages[ii].getFrom(); 
 
        if (messagecount < num){
          MailApp.sendEmail(sender, "Auto Reply", "Hi, 您好！这是一条自动回复短信！本短信由 Google Apps Script 自动发出。");
        }else if( (messages[messagecount - 1].getDate().getTime() - messages[messagecount - num].getDate().getTime()) > hours * 60 * 60 * 1000 ){
          MailApp.sendEmail(sender, "Auto Reply", "Hi, 您好！这是一条自动回复短信！本短信由 Google Apps Script 自动发出。");
        }
        messages[ii].markRead();
        messages[ii].moveToTrash();
 
      }
    }
  }
}
```

7、点击保存，在弹出的对话框中输出你要显示的名称，例如：autoReplier。再单击“调试”会提示你授权，你按提示授权即可。授权完后会提示没有找到文件之类的，不用管。

8、再次点击“调试”，如果没有任何提示说明脚本没有错误。你也可以在“查看” --> “日志” --> “Apps 脚本信息中心”中查看脚本运行状态。如果显示状态为已完成则表示脚本没有错误。

![5](https://www.moeelf.com/action/Watermark?mark=L3Vzci91cGxvYWRzLzIwMTkvMDUvNzA2MzQ2ODQ1LnBuZw==)

9、单击“修改” --> “当前项目的触发器” --> 右下角的“添加触发器”，按下图设置好保存即可。

![6](https://camo.githubusercontent.com/3be707e008523c041b765d2191fb5060f952654f/68747470733a2f2f7777772e6d6f65656c662e636f6d2f616374696f6e2f57617465726d61726b3f6d61726b3d4c33567a63693931634778765957527a4c7a49774d546b764d4455764e4445324d4459794e6a51354d693577626d633d)

10、好了，现在你可以给自己发一条短信试试了。  （有问题请留言）

本文转自[萌精灵](https://www.moeelf.com)原文地址：https://www.moeelf.com/archives/4.html
