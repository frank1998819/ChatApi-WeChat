# ChatApi-WeChat
微信聊天接口，可用于制作微信聊天机器人

## 这是什么？
* 这是一个模拟的微信聊天客户端
* 该客户端使用的接口来自于网页版微信

## 有何优点？
* 对接口和流程进行了封装，更加简单易用
* 暴露了一个监听器，可以自己实现监听器以开发自己的业务功能
* 几乎全部功能的支持
    * 监听文字、图像、语音、视频、文件、系统提示等消息
    * 发送文字和图像消息
    * 发送好友申请
    * 修改好友备注
    * 添加和移除群成员

## 接下来的功能
* 全功能支持
* 更加友好的Demo
* 健壮性加强

## 如何使用
* 首先下载必要的依赖，[gson](https://github.com/google/gson)和[xtools-common](https://github.com/xuxiaoxiao-xxx/XTools-Common)
* 以下是一个学别人说话的小机器人，用到了该库提供的大部分功能
```java

public class WeChatDemo {
    //网页微信登录时有两个重要的值（wxsid，wxuin）是在cookie中返回的，这里使用了默认的内存Cookie管理器
    public static final CookieManager cookieManager = new CookieManager();
    //新建一个模拟微信客户端，并绑定一个简单的监听器
    public static WeChatClient wechatClient = new WeChatClient(new WeChatClient.WeChatListener() {
        @Override
        public void onQRCode(String qrCode) {
            System.out.println("onQRCode:" + qrCode);
        }

        @Override
        public void onAvatar(String base64Avatar) {
            System.out.println("onAvatar:" + base64Avatar);
        }

        @Override
        public void onFailure(String reason) {
            System.out.println("onFailure:" + reason);
        }

        @Override
        public void onLogin() {
            System.out.println("onLogin");
            System.out.println(String.format("您有%d名好友、关注%d个公众号、活跃微信群%d个", wechatClient.uFriends().size(), wechatClient.uPublics().size(), wechatClient.uChatrooms().size()));
        }

        @Override
        public void onMessageText(String msgId, User userWhere, User userFrom, String text) {
            System.out.println("onMessage:" + text);
            //学习别人说话
            if (!userFrom.UserName.equals(wechatClient.uMe().UserName)) {
                wechatClient.mText(userWhere.UserName, text);
            }
        }

        @Override
        public void onMessageImage(String msgId, User userWhere, User userFrom, File image) {
        }

        @Override
        public void onMessageVoice(String msgId, User userWhere, User userFrom, File voice) {
        }

        @Override
        public void onMessageCard(String msgId, User userWhere, User userFrom, String userName, String nick, int gender) {
        }

        @Override
        public void onMessageVideo(String msgId, User userWhere, User userFrom, File thumbnail, File video) {
        }

        @Override
        public void onMessageOther(String msgId, User userWhere, User userFrom) {
        }

        @Override
        public void onNotify(AddMsg addMsg) {
        }

        @Override
        public void onSystem(AddMsg addMsg) {
        }

        @Override
        public void onLogout() {
            System.out.println("onLogout");
        }
    }, cookieManager, null, Level.ALL);

    public static void main(String[] args) {
        CookieHandler.setDefault(cookieManager);
        //启动模拟微信客户端
        wechatClient.startup();
        //查看模拟微信客户端是否正在运行
        //wechatClient.isWorking();
        //关闭模拟微信客户端
        //wechatClient.shutdown();
    }
}
```