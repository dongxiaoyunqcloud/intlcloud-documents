## 简介

在此实践中，用户可以在腾讯云对象存储（以下简称 COS）上托管静态网站，访客可以通过自定义域名（例如 `www.example.com` ）访问托管的静态网站。无论您是想在 COS 上托管已有静态网站还是从零开始建站，此实践可帮助您在 COS 上托管静态网站。以下是具体步骤：
![](https://main.qcloudimg.com/raw/d70261f2d960e9da84a08db561af9b30.png)

## 事前准备

以下是实践过程中，将会用到的相关服务：

- [域名注册](https://dnspod.cloud.tencent.com/?from=qcloudProductDns)：托管静态网站前，您需要先注册一个域名，例如 `www.example.com`。您可通过腾讯云 [域名注册](https://dnspod.cloud.tencent.com/?from=qcloud) 申请域名。
- [对象存储 COS](https://cloud.tencent.com/product/cos)：使用 COS 创建存储桶，上传的网页内容将存放到存储桶，对存储桶设置**公有读私有写**的访问权限（允许所有人查看网页内容）。
- [内容分发网络 CDN](https://cloud.tencent.com/product/cdn-scd)：结合 CDN 和云解析服务，使得域名和网站内容绑定的同时，还可以为静态网站加速，降低访问延迟，提高可用性。
- [云解析](https://cloud.tencent.com/product/cns?from=qcloudProductCns)：使用云解析，实现使用自定义域名访问静态网站的目的。

> ?本指南中的所有步骤都使用`www.example.com` 作为示例域名。实际操作中请使用您的自有域名替换此域名。

## 操作步骤

### 1. 注册域名与备案

域名注册是在互联网上建立任何服务的基础。注册域名之后，还需要进行备案，网站才能正常访问。请根据您的具体情况进行操作：

- 已注册域名并备案，可跳过本步骤，进行 [步骤2](#创建存储桶)。
- 已注册域名但未备案，请进行 [域名备案](https://cloud.tencent.com/product/ba)。
- 未注册域名，请先 [注册域名](https://dnspod.cloud.tencent.com/?from=qcloud)，再进行 [域名备案](https://cloud.tencent.com/product/ba)。

<span id="创建存储桶"></span>

### 2. 创建存储桶并上传内容

在完成域名注册及备案后，您需要在 COS 控制台中执行以下任务，以创建和配置网站内容：

- [创建存储桶](#create)
- [配置存储桶并上传内容](#upload)

<span id="create"></span>

#### 2.1 创建存储桶

请使用腾讯云账号登录 [COS 控制台](https://console.cloud.tencent.com/cos)，为您的网站创建相应的存储桶，存储桶用于存储数据，您可以将网站内容存储在一个存储桶中。如您首次使用 COS，可以通过控制台的概览界面直接创建存储桶，或者在【存储桶列表】导航栏创建存储桶，具体操作请参阅 [创建存储桶](https://cloud.tencent.com/document/product/436/13309) 文档。

<span id="upload"></span>

#### 2.2 配置存储桶并上传内容

1）将存储桶的访问权限设置为**公有读私有写**，使网站内容允许公开访问。操作详情请查阅 [设置访问权限](https://cloud.tencent.com/document/product/436/13315) 文档。

2）将您的网站内容上传到已创建好的存储桶。具体操作请参阅 [上传对象](https://cloud.tencent.com/document/product/436/13321) 文档。

在存储桶中存放的内容可以是文本文件、照片、视频——任何您想要托管的内容。如果您还未构建网站，则只需按此实践创建一个文件。

例如，您可使用以下 HTML 创建文件，并将其上传到存储桶。网站主页的文件名通常为 index.html。在后续步骤中，您将提供此文件作为网站的索引文档。

```shell
<!DOCTYPE html>
<html>
    <head>
        <title>Hello COS!</title>
        <meta charset="utf-8">
    </head>
    <body>
        <p>欢迎使用&nbspCOS&nbsp的静态网站功能。</p>
        <p>这是首页！</p>
    </body>
</html>
```

> !开启静态网站功能后，当用户访问任何不带文件指向的一级目录时，COS 默认优先匹配对应存储桶目录下 index.html，其次为 index.htm，若无此文件，则返回 404。

<span id="步骤3"></span>

### 3. 绑定自定义域名

> !
> - 用户只有绑定自定义域名并开启静态网站功能后，才可以直接在浏览器中预览内容。如使用默认域名（包括 CDN 加速域名和 COS 默认域名）访问资源时将始终弹出下载框。
> - 目前对象存储 COS 暂不支持添加不开启 CDN 的自定义域名，如需使用自定义域名，则需要开启 CDN 加速，在开启CDN时，须选择源站为**静态网站源站**，用户才能通过自定义域名直接显示页面。

可设置自定义域名直接指向存储桶，并开通静态网站功能，达到通过浏览器直接访问网站（存储桶中的内容）的目的。同时为降低网站访问延迟，提高可用性。在此实践中，将实现绑定自定义域名，同时也为自定义域名开启 CDN 加速，使网站访客获取更好的浏览体验。

#### 3.1 域名添加

在进行自定义域名添加时，有两种途径供您选择：

- [通过 CDN 控制台添加](#通过CDN控制台)：如果您想在添加自定义域名的同时，进行 CDN 高级管理和配置，可优先选择 CDN 控制台添加域名。如需了解其他配置，请参阅 [CDN 配置管理](https://cloud.tencent.com/document/product/228/6288)。
- [通过 COS 控制台添加](#通过COS控制台)：如果您只需要先添加自定义域名，不进行其他配置，可通过 COS 控制台快速添加。

<span id="通过CDN控制台"></span>

- **通过 CDN 控制台添加** 
  1）请登录 [CDN 控制台](https://console.cloud.tencent.com/cdn)，单击**域名管理**左侧导航，然后单击【添加域名】，进入**添加域名**界面。
  ![](https://main.qcloudimg.com/raw/d6b5ea180a5bfe57ff0d1acf5861206f.png)
  2）请输入需要加速的域名，源站类型选择**对象存储（COS ）**，存储桶设置选择托管网站内容的对应存储桶及其默认域名。业务类型选择**静态加速**，其他保持默认配置，提交即可。
  ![](https://main.qcloudimg.com/raw/15d1d5ed26b84b2dab8046ee7977469f.png)
  3）域名添加完成。
  a. 请关闭弹窗，耐心等待域名配置下发至全网节点（约5 ~ 10分钟）。
  ![](https://main.qcloudimg.com/raw/e352f9626cd1314615f8c8dd55b70211.png)
  b. 您的域名接入 CDN 后，系统会为您自动分配一个以 `.cdn.dnsv1.com` 为后缀的 CNAME 域名，CNAME 域名不能直接访问，您需要在域名服务提供商处完成 CNAME 配置，配置生效后，即可享受 CDN 加速服务。可在 CDN 控制台的 [域名管理页](https://console.cloud.tencent.com/cdn/access) 查看CNAME 记录。获取到 CNAME 记录后，再进行 [步骤 3.2](#步骤3.2)。
  ![CDN添加4](http://mc.qcloudimg.com/static/img/3922bf529760e262316381936e40e26c/image.png)

<span id="通过COS控制台"></span>

- **通过 COS 控制台添加**
  1）登录 [COS 控制台](https://console.cloud.tencent.com/cos5) ，进入左侧菜单栏【 存储桶列表】，单击存储网站内容的存储桶，进入存储桶。
  ![](https://main.qcloudimg.com/raw/e3fbbecd2816bb893bfbd5e739868a20.png)
  2）单击【域名管理】，进入域名管理页面，单击自定义域名下的【 添加域名】按钮，进入可配置状态。
  **域名**：输入您已购买的自定义域名（如 `www.example.com` ）
  **源站类型**：选择**静态网站源站**。
  然后单击【保存】即可完成添加。
  ![](https://main.qcloudimg.com/raw/7e642e83065c98fe80636432107abc4c.png)
  3）请稍等几分钟，等待域名部署上线完成后。然后复制对应的 CNAME 记录，再进行 [步骤 3.2](#步骤3.2)。
  ![](https://main.qcloudimg.com/raw/fbab10f4b29381a0c0af4838f9bfc3cf.png)

<span id="步骤3.2"></span>

#### 3.2 域名解析

1. 添加自定义域名后，还需进行域名解析。请登录 [云解析控制台](https://console.cloud.tencent.com/cns)，单击左侧菜单栏【域名解析列表】，进入域名解析列表。单击【添加解析】，弹出**添加域名**对话框。
   ![](https://main.qcloudimg.com/raw/f0603d93d1417277f79562aa98dbc0c4.png)
2. 输入自定义域名，单击【确定】保存即可。
   ![](https://main.qcloudimg.com/raw/878946d926cae79f7451afcbb129c1dd.png)
3. 域名添加成功后，单击域名，进入解析记录管理页面。单击【 添加记录】，弹出添加记录对话框。
   ![](https://main.qcloudimg.com/raw/deaec39dc4903ca72475a14d7373e034.png)
4. 记录类型选择 CNAME，主机记录留空，线路类型选择默认，填入在 [步骤 3](#步骤3) 获取的 CNAME 记录，TTL 保持默认，单击【确定】保存即可。完成解析添加后，大约需 15 分钟左右生效，请耐心等待。
   ![](https://main.qcloudimg.com/raw/1fb0076d1cebb05c4f58c9b61dd05044.png)

### 4. 开启静态网站功能

将网站内容与自定义域名绑定之后，需要开启 COS 的静态网站功能，才能通过浏览器直接访问网站内容。具体步骤请参阅 [设置静态网站](https://cloud.tencent.com/document/product/436/14984) 文档。

### 5. 测试验证

在完成上述步骤后，可通过在浏览器地址栏输入网站域名进行访问，来验证实践结果，以 `www.example.com` 为例：

- `http://www.example.com` ——返回名为 example 的存储桶中的索引页面（index.html）。
- `http://www.example.com/test.html`（不存在的文件） ——返回名为 example 的存储桶中的404文档（error.html）。

> ?在某些情况下，您可能需要清除浏览器缓存才能看到预期结果。
