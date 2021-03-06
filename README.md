### 前言

目前iOS App的内侧分发渠道很多，包括：蒲公英、fir.im等等三方分发平台，用户注册账号后，可以直接上传Archive后的ipa文件即可通过生成的应用下载页来下载安装。但是，为了满足个性化以及品牌宣传等需求，我们需要自建H5下载页，并配置好相关数据也可以制定自定义的分发下载渠道。

### 配置步骤

首先我们需要配置一个plist文件，包含2种尺寸的icon图标、app基本信息、和ipa文件的下载地址等，下面我们就以一个例子，介绍下具体的实现步骤：

1. Plist 文件
```
    # manifest-demo.plist 文件
    ....
    <array>
    				<dict>
    					<key>kind</key>
    					<string>software-package</string>
    					<key>url</key>
    					<string>https://com.download.ipa</string>		<!-- 这里填写ipa文件的下载地址 -->
    				</dict>
    				<dict>
    					<key>kind</key>
    					<string>display-image</string>
    					<key>url</key>
    					<string>https://com.icon.57x57.png</string>	<!-- 57x57 像素的icon图片 -->
    				</dict>
    				<dict>
    					<key>kind</key>
    					<string>full-size-image</string>
    					<key>url</key>
    					<string>https://com.icon.512x512.png</string> <!-- 512x512 像素的icon图片 -->
    				</dict>
    			</array>
    			<key>metadata</key>
    			<dict>
    				<key>bundle-identifier</key>
    				<string>com.app.xxx</string>	<!-- 包名 -->
    				<key>bundle-version</key>
    				<string>1.0.0</string>				<!-- 版本号 -->
    				<key>kind</key>
    				<string>software</string>
    				<key>title</key>
    				<string>appName</string>					<!-- 应用名称 -->
            <key>subtitle</key>	
    				<string>subName</string>				  <!-- 副标题 -->
    			</dict>
    ....
```

上面是一个plist配置文件的demo，可以直接在上面修改相关信息，也可以在Archive 打包的时候在Xcode 的提示框中勾选同时生成该plist文件。一般都采取直接修改现成的plist文件即可。

2. 下载链接

接下来，我们需要将上面配置好的plist文件移交给后台开发人员，进行服务器端配置。然后，前端需要将点击下载链接按照如下格式配置：

    itms-services://?action=download-manifest&url=https://app.download.xxx.com/install-manifest.plist

其中，最主要的是修改"url= '' 为刚才服务器端配置的plist文件下载地址，例如：https://app.download.xxx.com/install-manifest.plist

然后，在h5页面中将下载按钮的响应链接（例如：<a>标签）配置为上面的地址。例如：

    <a href="itms-services://?action=download-manifest&url=https://app.download.xxx.com/install-manifest.plist">立即下载</a>

3. 测试

基本的配置就这么多，现在我们可以在实际项目中进行测试了，当点击下载按钮的时候回弹出一个提示框："是否下载xxx应用?"，这里注意前提是需要先给这个ipa包进行企业签名才能进行公测分发。点击下载后，回到桌面即可看到应用已经在安装了。


