---

copyright:
  years: 2017, 2018
lastupdated: "2018-07-03"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:app_name: data-hd-keyref="app_name"}

# 入门教程
{: #getting_started}

* {: download}恭喜您，您已在 {{site.data.keyword.Bluemix}} 上部署了 Hello World 样本应用程序！要开始使用，请按照本逐步指南进行操作。或者，<a class="xref" href="http://bluemix.net" target="_blank" title="（下载样本代码）"><img class="hidden" src="../../images/btn_starter-code.svg" alt="下载应用程序代码" />下载样本代码</a>并自行探究。

按照此入门教程，设置开发环境，在本地以及在 {{site.data.keyword.Bluemix}} 上部署应用程序，并在应用程序中集成数据库服务。

在这些文档中，对 Cloud Foundry CLI 的引用现已更新为 {{site.data.keyword.Bluemix_notm}} CLI！{{site.data.keyword.Bluemix_notm}} CLI 具有与 Cloud Foundry CLI 相同的常见命令，但它可以与 {{site.data.keyword.Bluemix_notm}} 帐户和其他服务更好地集成。在本教程中，可以了解有关如何使用 {{site.data.keyword.Bluemix_notm}} CLI 的更多信息。
{: tip}

## 开始之前
{: #prereqs}

您将需要以下内容：
* [{{site.data.keyword.Bluemix_notm}} 帐户](https://console.bluemix.net/registration/)
* [{{site.data.keyword.Bluemix_notm}} CLI](../../cli/reference/bluemix_cli/download_cli.html)
* [Git ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://git-scm.com/downloads){: new_window}
* [PHP ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://php.net/downloads.php){: new_window}
* [Composer ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://getcomposer.org/download/){: new_window}

## 步骤 1：克隆样本应用程序
{: #clone}

首先，克隆存储库，并切换到样本应用程序所在的目录。

  ```
git clone https://github.com/IBM-Cloud/get-started-php
  ```
  {: codeblock}

  ```
cd get-started-php
  ```
  {: codeblock}

## 步骤 2：本地运行应用程序

1. 安装依赖项。

  ```
php composer.phar install
```
{: codeblock}

1. 在本地运行应用程序。

  ```
php -S localhost:8000
  ```
  {: codeblock}

1. 通过以下 URL 查看应用程序：http://localhost:8000

## 步骤 3：准备部署应用程序
{: #prepare}

如果要部署到 {{site.data.keyword.Bluemix_notm}}，设置 manifest.yml 文件会很有用。manifest.yml 包含有关应用程序的基本信息，例如名称、要为每个实例分配的内存量以及路径。我们在 `get-started-php` 目录中提供了样本 manifest.yml 文件。

打开 manifest.yml 文件，将 `name` 从 `GetStartedPHP` 更改为您的应用程序名称 <var class="keyword varname" data-hd-keyref="app_name">app_name</var>。
{: download}

  ```
 applications:
 - name: GetStartedPHP
   random-route: true
   memory: 128M
  ```
  {: codeblock}

在此 manifest.yml 文件中，**random-route: true** 会为应用程序生成随机路径，以避免路径与其他路径冲突。如果您愿意，可以将 **random-route: true** 替换为 **host: myChosenHostName**，以提供您选择的主机名。
{: tip}

## 步骤 4：部署应用程序
 {: #deploy}

可以使用 {{site.data.keyword.Bluemix_notm}} CLI 来部署应用程序。

1. 登录到 {{site.data.keyword.Bluemix_notm}} 帐户，然后选择 API 端点。

   ```
ibmcloud login
```
   {: codeblock}

  如果您有联合用户标识，请改用以下命令来使用单点登录标识进行登录。请参阅[使用联合标识登录](https://console.bluemix.net/docs/cli/login_federated_id.html#federated_id)以了解更多信息。
  ```
ibmcloud login --sso
  ```
  {: codeblock}

1. 设置目标 Cloud Foundry 组织和空间：

  ```	  
ibmcloud target --cf
  ```
  {: codeblock}

  如果未设置组织或空间，请参阅[添加组织和空间](https://console.bluemix.net/docs/account/orgs_spaces.html)。
  {: tip}

1. 从 *get-started-php* 目录中，将应用程序推送到 {{site.data.keyword.Bluemix_notm}}

   ```
ibmcloud cf push
   ```
   {: codeblock}

  这可能需要一分钟。如果部署过程中发生错误，可以使用命令 `ibmcloud cf logs <Your-App-Name> --recent` 进行故障诊断。

部署完成后，应该会看到一条消息，指示应用程序正在运行。可以通过 push 命令输出中列出的 URL 查看应用程序。还可以发出以下命令来查看应用程序状态和 URL。

  ```
ibmcloud cf apps
```
  {: codeblock}

## 步骤 5：添加数据库
{: #add_database}

接下来，我们要将 {{site.data.keyword.cloudant_short_notm}} NoSQL 数据库添加到此应用程序中，并设置此应用程序，使其可以在本地以及在 {{site.data.keyword.Bluemix_notm}} 上运行。

1. 在浏览器中，登录到 {{site.data.keyword.Bluemix_notm}}，然后转至“仪表板”。选择**创建资源**。
2. 选择**数据和分析**部分，选择 **{{site.data.keyword.cloudant_short_notm}}**，然后创建该服务。
3. 转至**连接**视图，选择应用程序，然后选择**创建连接**。
4. 出现提示时，选择**重新编译打包**。{{site.data.keyword.Bluemix_notm}} 将重新启动应用程序，并使用 `VCAP_SERVICES` 环境变量为应用程序提供数据库凭证。此环境变量仅可用于在 {{site.data.keyword.Bluemix_notm}} 上运行的应用程序。

通过环境变量，可以将部署设置与源代码分开。例如，可以将数据库密码存储在环境变量中，然后在源代码中引用此环境变量，而不是对密码进行硬编码。
{: tip}

## 步骤 6：使用数据库
{: #use_database}
现在，我们将更新本地代码以指向此数据库。我们将创建一个 .env 文件，用于存储应用程序要使用的服务的凭证。仅当应用程序在本地运行时，才会使用此文件。在 {{site.data.keyword.Bluemix_notm}} 中运行时，将从 VCAP_SERVICES 环境变量中读取凭证。

1. 在 `get-started-php` 目录中，创建名为 `.env` 的文件并包含以下内容：

  ```
  CLOUDANT_HOST=
  CLOUDANT_USERNAME=
  CLOUDANT_PASSWORD=
  ```
  {: codeblock}

2. 在浏览器中，转至 {{site.data.keyword.Bluemix_notm}} 仪表板，然后选择**_您的应用程序_ > 连接**。单击 {{site.data.keyword.cloudant_short_notm}} 菜单图标 (**&vellip;**)，然后选择**查看凭证**。

3. 将 `CLOUDANT_HOST`、`CLOUDANT_USERNAME` 和 `CLOUDANT_PASSWORD` 字段的值复制并粘贴到 `.env` 文件，然后保存更改。结果将类似于以下内容：

  ```
  CLOUDANT_HOST=abc...yz.cloudant.com
  CLOUDANT_USERNAME=abc...yz
  CLOUDANT_PASSWORD=445d...d1a
  ```
  {: codeblock}

4. 在本地运行应用程序。

  ```
php -S localhost:8000
  ```
  {: codeblock}

查看应用程序：http://localhost:8000。现在，您输入到应用程序中的所有名称都已添加到数据库。

本地应用程序和 {{site.data.keyword.Bluemix_notm}} 应用程序共享该数据库。可以通过上面 push 命令输出中列出的 URL 查看 {{site.data.keyword.Bluemix_notm}} 应用程序。在刷新浏览器后，从任一应用程序添加的名称都应该会同时出现在这两个应用程序中。

请记住，如果无需应用程序继续运行，请将其停止，这样就不会产生任何意外的费用。
{: tip}  

## 后续步骤

* [教程](/docs/tutorials/index.html)
* [样本 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://ibm-cloud.github.io){: new_window}
* [Architecture Center ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/cloud/garage/category/architectures){: new_window}
