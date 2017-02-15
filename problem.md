## pch

在使用.pch文件时要注意在Build Setting里设置时不能使用自己的固定路径（自己的路径放在了桌面，报错时要根据错误找到原因）在团队开发时使用自己的固定路径别人没法用，在head里设置$(SRCROOT)/路径.pch

## scrollView

> 在storyboard里使用scrollview时要考虑性能，比如往里面加button时不要直接把button加进来，应该先设置好约束，在scrollview里放view在view上加button，或者是单独撑出来一个带有xib文件的类，然后再和storyboard里相关控件关联，还有命名的时候要规范，尽量不要与系统的重名

## 程序进入后台之前如何保存窗口状态

[窗口状态](http://www.cnblogs.com/ljlkfx/archive/2015/05/26/4531268.html)

## github常用操作命令

[git命令](http://caibaojian.com/use-github.html)

## 快捷键

ctrl+shift+o  打开

ctrl+shift+w  关闭

ctrl+alt+esc 强制退出某个程序

在切图的时候：

1.  ctrl+shift+alt+s 用来切图。注意2倍图要用@2x；
2.  commond + m用来拉线量高度
3. commond + i 用来查看颜色
            
# storyboard

> 在使用storyboard时scrollview里加图片滑动，当只有一张图片时直接在scrollview里加imageview，会遇到图片不能占满占满整个scrollview的问题，需要在设置里把scale to fill改成top

## Scale:拉伸图片

- Aspect：图片长宽的比例，保持图形的长宽比，保持图片不变形。
- Aspect Fill：在保持长宽比的前提下，缩放图片，使图片充满容器。
- Aspect Fit：在保持长宽比的前提下，缩放图片，使得图片在容器内完整显示出来。
- Scale to Fill: 缩放图片,使图片充满容器。图片未必保持长宽比例协调，有可能会拉伸至变形。
                   
## 约束问题

  在约束的时候在下方的四个按钮选择第二个，在做完约束后选择all frame contarins ，也就是容器中的所有约束都更新，因为约束的时候有时根据其它的控件定的位置，需要全部更新

##代码规范

- 方法之间要空一行在写，方法源或者代理之类的要写promma注释，命名的时候要见其名只其意 
- 在打断点的时候在下面的日志输出里打 po +名字 ，可以查看该类的内容

##协议

协议可以继承，协议是对nsobject的扩展

##bug

在自定义的cell里加button，给button传tag值得时候不能直接传，要给cell传tag值，然后在delegate里把cell的tag传给button的tag

## 判断手机号
`+ (BOOL)checkPhoneNum:(NSString *)phone
{
NSString *pattern = @"^1+[34578]+\\d{9}";
NSPredicate *pred = [NSPredicate predicateWithFormat:@"SELF MATCHES %@", pattern];
BOOL isMatch = [pred evaluateWithObject:phone];
return isMatch;

}
`
##正则匹配用户密码6-18位数字和字母组合
`+ (BOOL)checkPassword:(NSString *) password

{
NSString
*pattern = @"^(?![0-9]+$)(?![a-zA-Z]+$)[a-zA-Z0-9]{6,18}";

NSPredicate
*pred = [NSPredicate predicateWithFormat:@"SELFMATCHES %@", pattern];

BOOL
isMatch = [pred evaluateWithObject:password];

return

isMatch;
}
`
##iOS中UILabel滚动字幕动画的实现
`UILabel *label = [[UILabel alloc] initWithFrame:CGRectMake(10, 100, 150, 50)];
label.text = @"this is how life goes";
UILabel *label2 = [[UILabel alloc] initWithFrame:CGRectMake(300, 100, 100, 50)];
label2.text = @"up and down";
[self.view addSubview:label];
[self.view addSubview:label2];
[UIView animateWithDuration:3.0 delay:0 options:UIViewAnimationOptionRepeat | UIViewAnimationOptionAutoreverse animations:^{
label.transform = CGAffineTransformMakeTranslation(80, 200);
label2.transform = CGAffineTransformMakeTranslation(-50, 200);
} completion:nil];
`
##微信登录问题

1.iOS 9系统策略更新，限制了http协议的访问，此外应用需要在“Info.plist”中将要使用的URL Schemes列为白名单，才可正常检查其他应用是否安装。
受此影响，当你的应用在iOS 9中需要使用微信SDK的相关能力（分享、收藏、支付、登录等）时，需要在“Info.plist”里增加如下代码：
<key>LSApplicationQueriesSchemes</key>
<array>
<string>weixin</string>
</array>
<key>NSAppTransportSecurity</key>
<dict>
<key>NSAllowsArbitraryLoads</key>
<true/>
</dict>

2.用Diplomat这个第三方

##根据字符串的长度设定label的高度和大小
`CGSize size = [self.label.text boundingRectWithSize:CGSizeMake(355, MAXFLOAT) options: NSStringDrawingUsesLineFragmentOrigin attributes:[NSDictionary dictionaryWithObject:self.label.font forKey:NSFontAttributeName] context:nil].size;`

##swift

case中的_代表任何值

##统计行数

`find . "(" -name "*.m" -or -name "*.mm" -or -name "*.cpp" -or -name "*.h" -or -name "*.rss" ")" -print | xargs wc -l`

##项目中的需求：控制弹窗弹出次数，要求每天弹出一次即可，写一个类，方便调用
`+(void)jumpToVC:(UIViewController *)myVC withSaveParam:(NSString *)saveParam withSaveDate:(NSDate *)saveDate withNavigationController:(UINavigationController *)nav{
//判断参数是否保存
if (saveParam.length>0 && saveParam != nil) {//Y
YSXLog(@"参数已保存");
}else{//N
//判断时间是否保存
if (saveDate != nil) {//Y
//判断是否超过24小时
if ([[NSDate date] timeIntervalSinceDate:saveDate]/3600 >24) {//超过24小时

[nav pushViewController:myVC animated:YES];
}else{
YSXLog(@"没有超过24小时");
}

}else{//N跳转

[nav pushViewController:myVC animated:YES];
}
}}`

###调用时，由于“所依赖的界面”还没加载完，所以有时不能成功弹出，可以适当延迟弹出时间1秒
`dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(1 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
EmailViewController * vc = [[EmailViewController alloc] init];
[YSXJumpToVC jumpToVC:vc withSaveParam:[YSXUserInfo sharedYSXUserInfo].addEmail withSaveDate:[YSXUserInfo sharedYSXUserInfo].addEmailDate withNavigationController:self.navigationController];
});`

###cocoapods遇到的坑（Could not read from remote repository）
url = https://server/username/*your*git*app*.git   （比如：url = https://hemcsec.tk/DEEP/myproject.git）
改完之后保存，重新git push -u origin master  问题解决。 

###检查是否安装地图的应用

`[[UIApplication sharedApplication] canOpenURL:[NSURL URLWithString:@"appurlscheme://"]`

[其它博客地址的记载](http://www.jianshu.com/p/c4169171eaa6)

###获取设备UUID的方法
[地址](http://blog.csdn.net/wsdxsyb/article/details/51773494)
