### mpvue + iview weapp 踩坑
1. 小程序中没有真实dom，mpvue所写的dom会被转换为 \<view> 标签，所以如下写法，文字不会一行显示，需要把文字信息也用dom包裹起来，重新书写样式

    ```html
        <div>
            <i class="iconfont"></i>test
        </div>
    ```
    
2. iview 中 i-modal 不能阻止小程序原生的滑动事件，需要添加catchtouchmove="true"在i-modal组件上

    ```html
      <i-modal
        catchtouchmove="true"
      >
    ```
3. 小程序button会默认带有边框，去除需要设置伪类

    ```css
        button::after{
            border: none;
        }
    ```
    
4. 小程序会把 img 标签转化为 image，所以直接给img添加css，在小程序中没有作用,可添加mode属性解决,参考[小程序image](https://developers.weixin.qq.com/miniprogram/dev/component/image.html)

    ```html
        <img class="wx_image" :src="item.headUrl" mode="widthFix"/>
    ```
    
5. mpvue 无法使用 /deep/ >>> 来修改 scoped下子组件样式
6. 慎用v-show，因为小程序dom原因，有时候书写的层级结构，解析出来并不是那样，所以v-show可能会对子级没有作用，可以使用v-if
7. 小程序不支持 promise finally
8. iview的index索引器给的item-height没用，修改过源码
9. 小程序富文本解析组件 mpvue-wxparse，h5不支持
10. iview的rate(评分)组件不支持自定义颜色，修改源码
11. 写css时避免使用标签，尽量使用class名称，小程序会把dom解析为view组件
12. 使用textarea组件时，如果父级是定位浮层之类的，需要给textarea绑定:fixed="true"

    ```html
    <textarea
        cursor-spacing="10px"
        confirm-type="send"
        :show-confirm-bar="false"
        :fixed="true"
    />
    ```
    
13. 如在微信小程序tabbar页面，全屏的弹窗浮层需要调用微信方法隐藏底部tabbar，

    ```js
      wx.hideTabBar({});
    ```
    
14. 在微信小程序可以使用@作为src文件路径

    ```js
      import foundList from "@/components/custom/found-list";
    ```
    
15. 使用 npm run build / dev 时，获取pages文件目录下的文件时，会按照字典顺序排序，如需要将指定目录放置第一位，可在目录名前加 _
16. 退出登录或axios拦截器中跳转到登录页时，使用

  ```js
    wx.redirectTo()
  ```
17. 可调用微信onPullDownRefresh方法来实现page页面下拉刷新

  ```js
    // 和methods方法同级
    async onPullDownRefresh() {
        // do something
        // 停止下拉刷新
        wx.stopPullDownRefresh();
      
    }
  ```

18. 动态设置微信小程序title

  ```js
   wx.setNavigationBarTitle({
      title: '测试标题'
   });
  ```
  
19. 小程序链接跳转不生效，如需要，则需要使用微信提供的web-view组件，并在小程序管理中配置白名单，iframe页面同样需要配置
20. 使用iconfont的字体文件时，可能会跟微信的冲突
21. 如文件大小超过微信限制，不能在手机预览时，可使用npm run build方式打包，并关闭productionSourceMap
22. 使用动态绑定的图片(如用户默认头像等)需要放到 static 目录下，vue开发同理
23. 输入框之类的组件，尽量使用原生的，iview的会有一些未知问题
24. iview组件样式或方法如有不合适，可自行修改