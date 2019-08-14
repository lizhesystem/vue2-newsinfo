## 基于vue2.0的商城资讯
***
- 使用Mint UI组件库
- 使用MUI框架

1.  scoped实现原理：通过css属性选择器，在上级div设置了一个选择器的属性。
2.  `bootstarp`属于代码片段。`mint.ui`是基于vue的移动端组件。

## 使用mint.ul的一个例子

- 导入mintUi和css，Vue.use

CSS组件直接加到APP组件里（因为已经全都导入样式了不需要导入）。

JS组件按需导入，在组件`script`里导入使用。

- 展示点击按钮后出现的图标例子，根据API模拟3秒消失的例子。
- 组件体积太大，按需导入-需要装个插件-然后修改.babelrc配置文件，这样做的好处是js文件小很多（开发阶段不关心，上线阶段再去关心）
- MUI-代码片段：提供好用的样式，**类似于bootstarp**，和bootstarp一样的导入方式。

## 开始

### one轮播图+6宫格+首页切换组件

1. 拷贝文件，执行`npm i`根据配置文件安装环境了，清理下路由和不需要的文件。
2. 按需导入，header和底部，一个是组件，一个是MUI，修改header的样式
3. 把项目托管到码云--记得把那几个不需要提交的文件标识下，密钥、地址之类的。git pust 什么的
4. 做底部小图标的时候，需要导入扩展的CSS样式，还有字体，还有扩展的标签。自己去MUI里去找一下。。
5. 更改4个人a标签改为<router-link> ，并且调成点击哪个就高亮显示，设置路由高亮！
6. 点击四个组件，设置不同的内容，实现切换功能。
7. 在第一个首页里增加轮播图功能，使用组件，按需导入。然后放入组件（默认样式高度是0需要调整），使用sass定义每个轮播图的背景。
8. 安装vue-resource -S 导入`vue-resource` Vue.use()  安装完毕后。就可以发送请求，在第一个首页里发请求，视频里是调用接口，然后返回数据循环渲染轮播图，由于没有接口地址，直接放图片导入把，到时候写例子的时候假装有图片。
9. 添加九宫格，从MUI里grid里找到元素，复制ui及以下的内容到首页组件，修改字体样式，样式用人家的属性，图片用自己的图片替换原来的字体图标。
10. 左边一边消失，右边进来，切换的时候。使用组件的动画效果，但是这里需要注意的是在使用动画的时候会有几个问题，更改属性`v-enter{ 0,100% }  v-enter-to{ 0 -100%}` 分开写，这样能实现从左边进来，从左边出去，而不是左边进来右边出去。出去会往下顿一下需要把出去的动画脱离文档流 absolute,这样不会往下占用文档流。

### two新闻标题+新闻内容
1. 创建news文件夹里面放新闻的组件，新闻标题和新闻内容
2. 新闻资讯页面制作,绘制页面，使用MUI中的 media-list.html ,样式需要调整下。
3. 使用vue-resource获取数据(created加载)，使用全局配置api地址,页面里调用的地址都直接用后缀不加'/'的形式写，这样地址改变只需要改一个配置即可，由于没有API。直接在组件里把title和content写死显示几条数据。
    + 坑1：路由跳转的时候页面是跳转了但是组件的数据加载不出来，看地址是没问题说明组件配置的没问题，后来检查发现是route-link跳转的地址写的不对` <router-link to="/HomeContainer/NewsLists">` 后面的NewsLists少写个s。
4. 把新闻的每一项都改造成route-link，用于路由的跳转，同时在跳转的时候需要提供参数编号：id，唯一的标识符，需要配置路由的时候做拼接。
5. 创建新闻详情的组件的NewInfo.vue,在路由模块里把新闻详情的路由地址和组件页面对应起来。
6. 渲染新闻详情，先写布局标题+时间+点击次数+br+内容，写样式，写完后使用vue-resource获取数据，**（由于没有接口，自己在data里写数据，根据data的数据去拿值放到页面上即可)**,获取的新闻内容是个html的内容，需要v-html渲染，如果图片看不清楚的话需要去掉scoped,因为所有的样式都是使用属性包裹，不会出现样式污染的情况。

### three评论内容+发表评论
1. 封装一个评论公共组件,因为很多地方都使用这个组件comment.vue。
2. 在需要使用comment组件的页面在手工导入comment组件。
    + 谁用这个组件直接在组件里`script`里导入然后`export default{}`定义components的组件。而不是去挂到Vue入口里。
3. 在父组件中，也就是谁嵌套的子组件里，使用`components`属性，将刚导入的组件注册为子组件。
4. 在评论组件里写样式，按钮使用mint-ui，剩下的写样式，完成评论的完整样式。
5. 评论的数据没有接口写到data里，api里需要2个参数，新闻ID还有页码，新闻ID的取值用到了组件数据的传递，由于父组件里已经有这个id了，可以使用从父组件往子组件里传值。使用props:[]来接受。这样使用接口调用数据的时候就可以使用这个传过来的值了this.id
    + 由于没有接口,这个地方没有体验到比较可惜,在父组件里定义的子组件绑定数据,子组件用`props:["id"]`接收, 根据这个值再去请求评论的数据.
6 .循环数据渲染页面，把内容使用插值表达式填充到页面中，如果获取的数据库为空使用三元表达式写个默认内容。其他的比如说楼层+1表示，时间记得使用转换器，姓名等。
7. 点击加载更多的处理，首先点击的时候绑定一个事件，时间里把请求的页码参数更改下，改为+1，这样就可以得到第二页的数据this.page+1 ,再去获取数据，就会拿到新的第二页的数据。
    + 由于没有接口,加载功能功能没法写了,思路是根据请求的参数来获取不同的数据，请求评论的时候使用`/api/getcomments/43?pageindex=1` 有个pageindex参数来获取第几页的数据，如果做的话定义
    + 定义pageIndex: 1, -- 默认展示第一页数据
    + 点击按钮 [加载更多] 触发方法，`this.pageIndex++` 然后再去调用获取评论的方法即可。
8. 为了防止新数据覆盖老数据，不能把内容直接=，应该使用数组的concat方法，拼接上新数组,`this.comments = this.comments.concat(result.body.message)`
9. 发表评论--把评论文本框做双向数据绑定。绑定发表评论的点击事件。
10. 点击发表评论之后，首先先把评论内容去首位判断一下，然后调用post请求去储存评论数据**由于没有接口，这里请求post的时候参数id可以取路由`$route.params.id`的值，然后定义全局的json的格式请求为true**，如果储存成功接口返回0成功后，封装一个评论对象内容姓名+时间+内容。放到评论数组的首部，然后清空数据绑定的msg。

### four图片分享
1. 改造图片分享的路由链接为对于的组件photos，实现可以跳转。
2. 顶部的滑动条制作，使用mui里的`tab-top-webview-main`样式，a外面3跟div，去掉一个mui-fullscreen的全屏类(默认这个类是全屏显示)。
    + 滑动条无法触发滑动，通过检查官方文档发现这个是个JS的组件，需要初始化，1：导入mui.js，2：调用官方提供的方式进行初始化，3：`mui('.mui-scroll-wrapper').scroll({  deceleration: 0.0005 }) 系数越大滚动速度越慢，滚动距离越小`
    + 初始化的时候控制台报错，说`may not be accessed on sttrict mode` 在严格模式下一些方法不能用，推测后发现webpack打包好的bundle.js中默认启用了严格模式，导入的mui.js不是严格模式。冲突了
    + 解决方法：1：把mui.js中的非严格模式代码改掉，但是不行现实，2：把webpack打包时候的严格模式禁用掉。使用一个插件来搞定，使用`babel`的一个插件`babel-plugin-transform-remove-strict-mode` https://github.com/genify/babel-plugin-transform-remove-strict-mode 具体使用看readme
    + 滑动的时候报警告：Unable to preventDefault inside passive event listener due to target being treated as passive. See https://www.chromestatus.com/features/5093566007214080 解决方法，可以加上* { touch-action: none; } 这句样式去掉。
    + 第一次刚进入图片分享页面后滑动条不会滑动，如果要初始化，必须等DOM元素加载完毕，这个时候我们最好把方法写到`mounted()`里，因为这个时候DOM元素是最新的。因为这个时候DOM结构以及渲染好放到页面了
    + 滑动条调试好后，发下底部tabbar无法正常工作，发现是因为tabbar的样式导致的，这个时候把mui里原始定义的tabbar的样式`mui-tab-item`所有用到这个属性的都给改个名字重新定义下。在APP组件里
3. 渲染分类别表的数据，接口获取数据（没有接口自己写数据循环），自己处理一个全部的分类。ID为0，默认选中变颜色，mui提供了一个样式使用三元表达式来定义选中的时候变色`:class=[xxx,如果id=0显示或者不显示]`
4. 制作图片区域列表，懒加载使用mint-ui的`lazy load`指令，渲染图片数据，导入数据，根据文档给img家v-lazy指令，需要先定义图片列表的数据，使用本地图片（根据接口返回的数据定于数据）
    
3. 



