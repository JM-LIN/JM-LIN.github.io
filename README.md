> 注：由于版本的差异，以下操作不一定适用于所有人

# 制作
## 文章添加居中模块
```
<blockquote class="blockquote-center">工欲善其事，必先利其器</blockquote>
```

## 添加视频
```
<iframe height=100 width=100 src="视频地址">
```
长宽可以自己定义

## 添加 gif 
```
<iframe height=100 width=100 src="gif图片地址">
```
长宽可以自己定义


# 主题优化

## 制作站点LOGO
> 去[Favicon](http://tool.lu/favicon/)制作32*32的.ico图片
放在hexo/source/images/下

## 随机背景
> hexo\themes\next\source\css\ _custom\custom.styl添加代码
```styl
body {
    background:url(https://source.unsplash.com/random/1600x900);
    background-repeat: no-repeat;
    background-attachment:fixed;
    background-position:50% 50%;
}
```

## 背景动画
> hexo/themes/next/layout/_layout.swig 第123行

## 页面点击小红心
> hexo/themes/next/layout/_layout.swig 第126行

## 主题背景
> hexo/themes/next/source/css/_common/outline/outline.styl
在body函数添加

```styl
background: #F0F0F0;                            // 背景色
background: url（'/images/background.jpg');     // 背景图片
```

## 文章链接唯一化
    $ npm install hexo-abbrlink --save
> hexo/_config.yml
```yml
permalink: posts/:abbrlink/  # “posts/” 可自行更换
```
添加代码
```yml
# abbrlink config
abbrlink:
  alg: crc32  # 算法：crc16(default) and crc32 
  rep: hex    # 进制：dec(default) and hex
```

## 博文置顶
> 将hexo/node_modules/hexo-generator-index/lib/generator.js内所有代码替换为
```js
'use strict';
var pagination = require('hexo-pagination');
module.exports = function(locals){
  var config = this.config;
  var posts = locals.posts;
    posts.data = posts.data.sort(function(a, b) {
        if(a.top && b.top) { // 两篇文章top都有定义
            if(a.top == b.top) return b.date - a.date; // 若top值一样则按照文章日期降序排
            else return b.top - a.top; // 否则按照top值降序排
        }
        else if(a.top && !b.top) { // 以下是只有一篇文章top有定义，那么将有top的排在前面（这里用异或操作居然不行233）
            return -1;
        }
        else if(!a.top && b.top) {
            return 1;
        }
        else return b.date - a.date; // 都没定义按照文章日期降序排
    });
  var paginationDir = config.pagination_dir || 'page';
  return pagination('', posts, {
    perPage: config.index_generator.per_page,
    layout: ['index', 'archive'],
    format: paginationDir + '/%d/',
    data: {
      __index: true
    }
  });
};
```
在 hexo/scaffolds/post.md中增加 top: 0 (top值越大文章越靠前)

## 添加顶部加载条
> hexo/themes/next/layout/_partials/head.swig, 在maximum-scale=1”/>后添加代码（颜色自己选择）
```swig
<script src="//cdn.bootcss.com/pace/1.0.2/pace.min.js"></script>
<link href="//cdn.bootcss.com/pace/1.0.2/themes/pink/pace-theme-flash.css" rel="stylesheet">
<style>
    .pace .pace-progress {
        background: #1E92FB;            /*进度条颜色*/
        height: 3px;
    }
    .pace .pace-progress-inner {
         box-shadow: 0 0 10px #1E92FB, 0 0 5px     #1E92FB; /*阴影颜色*/
    }
    .pace .pace-activity {
        border-top-color: #1E92FB;    /*上边框颜色*/
        border-left-color: #1E92FB;    /*左边框颜色*/
    }
</style>
```

## 添加热度
> hexo/themes/next/layout/_macro/post.swig
在”leancloud-visitors-count”>标签后面添加℃。
然后打开 hexo/themes/next/languages/zh-Hans.yml，将visitors内容改为热度即可。


## 文章加密访问
> hexo/themes/next/layout/_partials/head.swig 在meta标签后面插入
```swig
<script>
    (function(){
        if('{{ page.password }}'){
            if (prompt('请输入文章密码') !== '{{ page.password }}'){
                alert('密码错误！');
                history.back();
            }
        }
    })();
</script>
```
在 hexo/scaffolds/post.md中增加 password:

## 文章内链接文本样式
> hexo\themes\next\source\css\_custom\custom.styl添加
```styl
.post-body p a {
  color: #0593d3;
  border-bottom: none;
  &:hover {
    color: #0477ab;
    text-decoration: underline;
  }
}
```

## 搜索功能
安装hexo-generator-searchdb
    $ npm install hexo-generator-searchdb --save
> 在hexo/_config.yml
```yml
search:
  path: search.xml
  field: post
  format: html
  limit: 10000
```
> 在hexo/themes/next/_config.yml
```yml
# Local search
local_search:
  enable: true
```

## 主页文章添加阴影效果
> hexo/themes/next/source/css_custom/custom.styl添加
```styl
.post {
   margin-top: 60px;
   margin-bottom: 60px;
   padding: 25px;
   -webkit-box-shadow: 0 0 5px rgba(202, 203, 203, .5);
   -moz-box-shadow: 0 0 5px rgba(202, 203, 204, .5);
  }
```

## 文章底部版权信息
> hexo/themes/next/layout/_macro/下添加my-copyright.swig
```swig
{% if page.copyright %}
<div class="my_post_copyright">
  <script src="//cdn.bootcss.com/clipboard.js/1.5.10/clipboard.min.js"></script>
  <!-- JS库 sweetalert 可修改路径 -->
  <script type="text/javascript" src="http://jslibs.wuxubj.cn/sweetalert_mini/jquery-1.7.1.min.js"></script>
  <script src="http://jslibs.wuxubj.cn/sweetalert_mini/sweetalert.min.js"></script>
  <link rel="stylesheet" type="text/css" href="http://jslibs.wuxubj.cn/sweetalert_mini/sweetalert.mini.css">
  <p><span>本文标题:</span><a href="{{ url_for(page.path) }}">{{ page.title }}</a></p>
  <p><span>文章作者:</span><a href="/" title="访问 {{ theme.author }} 的个人博客">{{ theme.author }}</a></p>
  <p><span>发布时间:</span>{{ page.date.format("YYYY年MM月DD日 - HH:MM") }}</p>
  <p><span>最后更新:</span>{{ page.updated.format("YYYY年MM月DD日 - HH:MM") }}</p>
  <p><span>原始链接:</span><a href="{{ url_for(page.path) }}" title="{{ page.title }}">{{ page.permalink }}</a>
    <span class="copy-path"  title="点击复制文章链接"><i class="fa fa-clipboard" data-clipboard-text="{{ page.permalink }}"  aria-label="复制成功！"></i></span>
  </p>
  <p><span>许可协议:</span><i class="fa fa-creative-commons"></i> <a rel="license" href="https://creativecommons.org/licenses/by-nc-nd/4.0/" target="_blank" title="Attribution-NonCommercial-NoDerivatives 4.0 International (CC BY-NC-ND 4.0)">署名-非商业性使用-禁止演绎 4.0 国际</a> 转载请保留原文链接及作者。</p>  
</div>
<script> 
    var clipboard = new Clipboard('.fa-clipboard');
    clipboard.on('success', $(function(){
      $(".fa-clipboard").click(function(){
        swal({   
          title: "",   
          text: '复制成功',   
          html: false,
          timer: 500,   
          showConfirmButton: false
        });
      });
    }));  
</script>
{% endif %}
```

> hexo/themes/next/source/css/_common/components/post/下添加my-post-copyright.styl
```styl
.my_post_copyright {
  width: 85%;
  max-width: 45em;
  margin: 2.8em auto 0;
  padding: 0.5em 1.0em;
  border: 1px solid #d3d3d3;
  font-size: 0.93rem;
  line-height: 1.6em;
  word-break: break-all;
  background: rgba(255,255,255,0.4);
}
.my_post_copyright p{margin:0;}
.my_post_copyright span {
  display: inline-block;
  width: 5.2em;
  color: #b5b5b5;
  font-weight: bold;
}
.my_post_copyright .raw {
  margin-left: 1em;
  width: 5em;
}
.my_post_copyright a {
  color: #808080;
  border-bottom:0;
}
.my_post_copyright a:hover {
  color: #a3d2a3;
  text-decoration: underline;
}
.my_post_copyright:hover .fa-clipboard {
  color: #000;
}
.my_post_copyright .post-url:hover {
  font-weight: normal;
}
.my_post_copyright .copy-path {
  margin-left: 1em;
  width: 1em;
  +mobile(){display:none;}
}
.my_post_copyright .copy-path:hover {
  color: #808080;
  cursor: pointer;
}
```

> 修改hexo/themes/next/layout/_macro/post.swig
```swig
<div>
      {% if not is_index %}
        {% include 'wechat-subscriber.swig' %}
      {% endif %}
</div>
```
在上面的代码块前添加以下代码
```swig
<div>
      {% if not is_index %}
        {% include 'my-copyright.swig' %}
      {% endif %}
</div>
```

> 在hexo/themes/next/source/css/_common/components/post/post.styl文件最后一行添加代码
```styl
@import "my-post-copyright"
```
在 hexo/scaffolds/post.md中增加copyright: true


## 隐藏网页底部powered By Hexo和强力驱动
> hexo/themes/next/layout/_partials/footer.swig
隐藏代码

```swig
{% if theme.copyright %}
<div class="powered-by">
  {{ __('footer.powered', '<a class="theme-link" href="http://hexo.io" rel="external nofollow">Hexo</a>') }}
</div>

<div class="theme-info">
  {{ __('footer.theme') }} -
  <a class="theme-link" href="https://github.com/iissnan/hexo-theme-next" rel="external nofollow">
    NexT.{{ theme.scheme }}
  </a>
</div>
```

## 添加RSS订阅源
    $ npm install hexo-generator-feed --save
> hexo\themes\_config.yml
```yml
rss: /atom.xml
```

## 博客压缩优化
    npm install hexo-neat --save
在hexo/_config.yml添加代码
```yml
# hexo-neat
neat_enable: true

neat_html:
  enable: true
  exclude:
  
neat_css:
  enable: true
  exclude:
    - '*.min.css'

neat_js:
  enable: true
  mangle: true
  output:
  compress:
  exclude:
    - '*.min.js'
```


# 侧边栏头像为圆形

> hexo/themes\next\source\css\_common\components\sidebar\sidebar-author.styl
替换函数.site-author-image

```styl
.site-author-image {
  display: block;
  margin: 0 auto;
  max-width: 96px;
  height: auto;
  border: 2px solid #333;
  padding: 2px;
  border-radius: 50%;
}
```

## 添加鼠标停留在头像上发生旋转效果
> 在.site-author-image函数中的border-radius: 50%;下添加

```styl
  webkit-transition: 1.4s all;
  moz-transition: 1.4s all;
  ms-transition: 1.4s all;
  transition: 1.4s all;
```
并添加函数
```styl
.site-author-image:hover {
  background-color: #55DAE1;
  webkit-transform: rotate(360deg) scale(1.1);
  moz-transform: rotate(360deg) scale(1.1);
  ms-transform: rotate(360deg) scale(1.1);
  transform: rotate(360deg) scale(1.1);
}
```

# 打赏

## 打赏功能
> hexo/next/_config.yml

```yml
# 打赏功能
reward_comment: 要是觉得不错，就鼓励一下吧！
wechatpay:  /images/Wechatpay.png
alipay:  /images/Alipay.png
```

## 打赏字体不闪动
> hexo/next/source/css/_common/components/post/post-reward.styl

```styl
/* 注释文字闪动函数
 #wechat:hover p{
    animation: roll 0.1s infinite linear;
    -webkit-animation: roll 0.1s infinite linear;
    -moz-animation: roll 0.1s infinite linear;
}
 #alipay:hover p{
   animation: roll 0.1s infinite linear;
    -webkit-animation: roll 0.1s infinite linear;
    -moz-animation: roll 0.1s infinite linear;
}
*/
```




**注**：本文内容来自互联网整理，将持续收集更新，欢迎留言补充！
