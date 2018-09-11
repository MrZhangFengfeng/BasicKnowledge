## 如何唤醒APP

### 常见的出现场景

- 浏览器--->唤醒--->APP
- 微信、QQ--->唤醒--->APP

### 唤醒APP的几种解决方案
1. URL Schema方式
- 条件：
    - APP需要注册自己的URL Schema，用来标识一个APP
    - Schema格式: `<schema域名>://<path>?<params>=<value>`。例如：
    `ctrip://wireless/hotel_inland_list?key=value`

- 代码：
    - iframe方式

            var _iframe = document.createElement('iframe');
            _iframe.src = schema;
            _iframe.style.display = 'none';
            document.body.appendChild(_iframe);
            window.setTimeout(function(){
                document.body.removeChild(_iframe);
                if((+new Date()) - openTime > 2500) {
                    window.location.href = url;
            }
            }, 2000);
    
    - a链接方式

            <a href="<schema域名>://<path>?<params>=<value>">打开APP</a>
    
    - location跳转

            window.location.href = "<schema域名>://<path>?<params>=<value>"

- 优缺点

    - 优点：iOS、Android均支持，开发简单；web-native协议制定简单。

    - 缺点：
    由于要考虑用户没有安装App的情况，所以当用户没有安装时，通过延迟会跳转到AppStore。iOS9+当跳转App时，会弹出一个弹框，让用户选择是否跳转，此时还在当前页，setTimeout中的代码会继续执行，导致用户还没来得及选择，就已经跳到AppStore。
    若用户未安装App，Android上schema打开失败，没有任何提示，延迟之后，跳下载页。但是iOS9+会先弹出个万恶的跳转失败的弹窗，延迟之后，再跳下载页。

    - 支持情况：URL schema方式一直被广泛使用，但是有些App并不认可，比如：微信、手机百度会在在客户端内拦截了schema方式呼端，导致URL schema方式在微信、手机百度中彻底失效。当然微信是存在一个白名单的，对于白名单中的分享链接是不会屏蔽schema调用的。

    - 兼容性：
        - **Android系统**：Chrome for Android无法通过iframe方式来调用schema，而通过a链接的方式可以成功调用，而针对Chrome内核的浏览器如360浏览器，对于iframe和a链接的方式都能支持，所以对Chrome内核的浏览器采用a链接的方式来调用schema；对于其他浏览器，如UC，QQ浏览器则采用iframe方式调用schema。

        - **iOS系统**：Safari浏览器不支持 iframe可直接做页面跳转；对于UC、Chrome、QQ只能通过a链接方式调用schema。上述提到的屏蔽schema方式的App：唤醒失败跳下载页。

IOS9+跳转失败弹窗：

![](https://upload-images.jianshu.io/upload_images/2982077-9acccf0abdac1a1e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/507/format/webp)


2. Universal Links

- 条件：
    - iOS9+系统
    - Universal Link是通过标准的http/https协议链接唤起App；若未安装App，访问此通用链接，可以自定义页面

- 优点：
    - 唯一性：不像自定义的Scheme方式，该方式是通过标准的http/https协议的链接到你的App.未安装App时，自定义的Scheme方式无法打开，而http/https的Universal Link通用链接有很好的兼容性；
    - 安全性：当用户安装了App时，iOS会从网站上去下载说明文件（下面所说的apple-app-site-association的文件，说明这个App可以打开哪些http链接）,这个说明文件是只有开发者有权限写并上传到网站的根目录，所以App与网页http链接的关联是安全的。
    - 可变性：当用户没有安装App时，Universal Link链接也能正常打开，也可以在回调中自定义在safari中打开或者去下载等行为。
    - 简单性：一个URL链接，可以将网站和App做关联。
    - 私有性：其他App在不知道你是否安装的情况下，也可以和你的App进行相互通信（因为只是一个普通的http链接啊）。其他APP无法通过Universal Links来判断是否安装了该APP。

- 缺点：
    - 只支持 iOS9+系统。
    - 在开启了Universal Link之后，用户可以通过该方式唤醒APP，成功唤醒后右上角会显示链接地址（iOS9、 iOS10会出现面包屑导航，iOS11+没有面包屑导航），当用户一不小心手动点击这个地址时（俗称：Universal link误关），Universal Link会失效（失效后，Universal Link唤醒就永远失败了，永远唤醒不了，跳下载页，所以我们要重新激活Universal Link）需要引导用户到Safari浏览器，引导用户点击Safari内置顶部的系统唤醒导航条，重新激活Universal Link，也可以将链接复制到备忘录中，长按链接出现用App打开，也可激活Universal Link。

- 开启Universal Links开关
    - 注册一个域名并且支持https，且https使用有效的证书（是私密链接）
    - 有权限上传到网站根目录.well-know目录下并且创建一个名字为apple-app-site-association的json文件
    - apple-app-site-association一定要传到域名根目录下
    - apple-app-site-association文件不能带后缀，务必把".json"的后缀去掉。
    - 大小写敏感；严格匹配配置的path路径。
    - apple-app-site-association文件的读取，是在刚安装的时候，不是在第一次打开APP的时候；iOS9.2开始，在相同的domain下，Universal Link不work，必须跨域；

3. Android Intent 方式

- 支持情况：
    - IOS：不支持
    - Android：Intent方式比URL Scheme方式要靠谱，能通过参数设置唤醒失败要跳转的地址；若手机能匹配到对应App，则成功呼到对应页面，若未安装，会跳手机自带的应用市场下载。


- - - -
## 一个唤醒解决方案示例

        //IOS9+开启Universal Links开关前提下，优先使用Universal Link进行唤醒
        if(UlLinkOpen && isIOS && ISOVersion >=9) {
            if(ulLink) {
                window.location.href = ulLink
            }
        }
        // 不支持intent唤醒的使用schema唤醒
        else if(!canIntent) {
            //IOS9+ 以及chrome浏览器不支持iframe方式
            if(getType.getUaType() == "safari" || getType.getUaType() == "chrom"){
                window.location = appUrl;
            }else {
                iframe(appUrl)
            }
            //如果没有安装APP，要设置一个延迟进行自定义(eg.下载)
            setTimeout(()=>{
                gotoDownLoad()...
            },延迟时间)
        }
        //支持intent唤醒
        else {
            intent(appUrl)
        }

#### 注意：
上面代码里的未安装APP的这个延长时间的设置很关键！延长时间的设定需要考虑：如果延长时间小于App的启动时间，App还未启动，就执行setTimeout代码；如果延长时间较长，当用户未安装App时，需要等待特别久的时间才能执行setTimeout代码。

#### 优化：
- 实际测试时发现：当成功呼起App时，用户再次返回到Safari浏览器的页面时已经跳转到下载页面了，此时需要对setTimeout做清除定时器处理。
- 当本地App被唤起时，App处于设备可视窗口的最高层，此时浏览器进入后台程序页面会被隐藏掉，会触发pagehide与visibilitychange事件，此时应先清除setTimeout事件；同时，document.hide属性设置为true，所以setTimeout内不做跳转处理，防止页面跳转到下载页面。
- 实际开发中，为了防止某些浏览器不支持这个 Page Visibility API，最好同时监听pagehide事件，这样会比较保险（相关代码如下）。

        //页面隐藏时触发
        window.onpagehide= function() {
            if(timeout) {
                clearTimeout(timeout)
            }
        }

        //页面的可见状态变化时触发
        visibilitychange= function() {
            const flag = document.hidden || document.webkitHidden
            if(flag && timeout) {
                clearTimeout(timeout)
            }
        }

        document.addEventListener('visibilitychange',visibilitychange,false)
        document.addEventListener('webkitvisibilitychange',visibilitychange,false)

- - - -
> 2018年1月7日，微信封掉了内置浏览器里的Universal Link呼端，导致微信内引导呼端的流程彻底中断

- 首先：我们有一个思路,既然微信容器内不能使用Universal Link呼端，那么我们可以脱离微信容器，做一个中转页面，引导用户去浏览器呼端。
- 其次：想想用户之前习惯性操作都是一次性直接点击呼端成功的，引导的话链路有点长，用户难免会吐槽。
- 所以，针对中转页面iOS客户端新增 Universal Link 匹配规则（以中转页面链接为主），以便在微信内，「使用浏览器打开」后，被系统拦截后可以直接打开客户端，不用引导去浏览器。
当然，对于Android用户，我们考虑到也可以引导用户去应用宝进行唤醒。

