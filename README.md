uEditor_django
==============

> 这个demo主要解决了django中集成uEditor,图片、视频、文件上传问题

##目前支持功能：
 - 基本文字、排版等功能
 - 图片上传、文件上传、视频上传功能
 - 在线文件、在线图片功能

##*未实现功能：*

 - *涂鸦功能*
 - *网络图片功能*

功能已在CentOS下，部署到nginx下，实测，可用。

使用方法：
-----

 1. 下载这里的完整代码，直接cd到根目录，运行`python manage.py runserver 1989`,可直接查看效果演示。

 2. 在urls.py中将uEditor所在目录配置成静态文件路径，本demo中为UE
    <pre><code>( r'^UE/(?P<path>.*)$','django.views.static.serve',
            { 'document_root':os.path.dirname(__file__).replace('\\','/')+"/UE"}),</code></pre>
 3. 将demo中ueconfig.json文件拷贝到自己项目的根目录中，并修改其中几处关键位置：
    <code>将"imageUrlPrefix": "/upload/images/"修改为自己项目中图片上传后保存的位置，demo中是/upload/images/这个目录
    将"scrawlUrlPrefix": "/upload/images/", 修改为自己项目中涂鸦
    "snapscreenUrlPrefix": "/upload/images/", 截图保存位置
    "catcherUrlPrefix": "/upload/images/", 网络图片保存位置
    "videoUrlPrefix": "/upload/vedio/"   视频文件保存地址
    "fileUrlPrefix": "/upload/files/" 附件保存地址
    "imageManagerUrlPrefix": "/upload/onlineimages/", 在线图片所在位置，在线图片实际就是服务器为用户提供的可选图片
    "fileManagerUrlPrefix": "/upload/onlinefiles/"  在线附件所在位置，在线附件实际就是服务器为用户提供的可选附件</code>
 4. json文件修改后，要把上面设置的路径设置为静态资源目录，例如demo中全部保存到/upload/的子目录下，那么在urls.py中配置如下：
 <pre><code>( r'^upload/(?P<path>.*)$', 'django.views.static.serve',{ 'document_root': (os.path.dirname(__file__)+"/upload").replace('\\','/') }),</code></pre>之后，确保子目录是存在的，为了方便，程序里没有自动创建目录的方法，需要手工创建，例如demo中创建了images、vedio、
files、onlinefiles、onlineimages几个子目录

 5. 将demo中的controller.py文件拷贝到项目中任意位置，其实controller就是一个异步处理的视图，拷贝完成后，在urls.py中配置相应的路由，demo中放到了根目录，所以配置如下：
`url(r'ueEditorControler','ueEditor_django.controller.handler')`
自己的项目中只需要将`ueEditor_django.controller.handler`改为`xxxx.controller.handler`即可

 6. 配置工作最后一步，将`ueditor.config.js`文件的  `, `serverUrl: URL + "/net/controller.ashx"  修改为 `, serverUrl: "/ueEditorControler"`  即上一步配置的url路由

至此，配置工作完成，剩下的就是到页面上引用uEditor了，下面是一个简单的html页面，可根据uEditor放置位置调整脚本
和样式的引用路径

    <html lang="en" xmlns="http://www.w3.org/1999/xhtml">
    <head>
        <link rel="stylesheet" type="text/css" href="/UE/third-party/SyntaxHighlighter/shCoreDefault.css">
    <script type="text/javascript" src="/ueEditor/third-party/SyntaxHighlighter/shCore.js"></script>
    <script type="text/javascript" charset="utf-8" src="/UE/ueditor.config.js"></script>
    <script type="text/javascript" charset="utf-8" src="/UE/ueditor.all.min.js"> </script>
    <script type="text/javascript" charset="utf-8" src="/UE/lang/zh-cn/zh-cn.js"></script>
    
    <script type="text/javascript">
    var ue = UE.getEditor('editor');
    SyntaxHighlighter.all();
    </script>
    </head>
    <body>
        <script id="editor" type="text/plain" style="width:auto;height:500px;"></script>
    </body>
    </html>
