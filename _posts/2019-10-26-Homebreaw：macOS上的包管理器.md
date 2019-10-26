---
layout: post
title: "Homebrew：macOS上的最受欢迎的包管理器"
bigimg: /img/big-imgs/big-img-07.jpg
date: 2019-10-26 20:46 +0800
tags: ["macOS", "Homebrew"]
categories: 利其器
---

### 安装Homebrew前的准备

在macOS上安装Homebrew之前，需要先将Xcode Command Line Tools安装完成，这样你就可以使用基于 Xcode Command Line Tools 编译的 Homebrew。

### 安装Homebrew

将以下命令粘贴至终端。

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

脚本会在执行前暂停，并说明它会将什么。更多高级安装选项请阅读[安装选项](https://docs.brew.sh/Installation)。在[Linux 和Windows上安装Homebrew](https://docs.brew.sh/Homebrew-on-Linux)。

安装完成后，运行以下命令，确保brew运行正常

```shell
$ brew doctor
```

### Homebrew能做什么

1. 使用Homebrew安装Apple（或Linux系统）没有预装但`你需要的东西`。比如你要安装wget。

   ```shell
   $ brew install wget
   ```

   

2. Homebrew将软件包安装至独立目录，并将其文件软链至`/usr/local`。

   ```shell
   $ cd /usr/local
   $ find Cellar
   Cellar/wget/1.16.1
   Cellar/wget/1.16.1/bin/wget
   Cellar/wget/1.16.1/share/man/man
   
   $ ls -l bin
   bin/wget -> ../Cellar/wget/1.16.1/bin/wget	
   ```

   

3. Homebrew不会将文件安装到它本身目录之外，你可把Homebrew安装到任务你喜欢的位置。

4. 轻松创建你自己的Homebrew包

   ```shell
   $ brew create https://foo.com/bar-1.0.tgz
   Created /usr/local/Homebrew/Library/Taps/homebrew/homebrew-core/Formula/bar.rb
   ```

5. 完全基于Git 和 Ruby，所以自由修改的同时，你可以轻松恢复你的修改或与上游的更新合并。

   ```shell
   $ brew edit wget	
   ```

6. Homebrew的配方（formulae）都是简单的Ruby脚本

   ```ruby
   class Wget < Formula
     homepage "https://www.gnu.org/software/wget/"
     url "https://ftp.gnu.org/gnu/wget/wget-1.15.tar.gz"
     sha256 "52126be8cf1bddd7536886e74c053ad7d0ed2aa89b4b630f76785bac21695fcd"
   
     def install
       system "./configure", "--prefix=#{prefix}"
       system "make", "install"
     end
   end
   ```

7. Homebrew使macOS（或你的Linux系统）更完整，使用`gem`安装RubyGems，使用`brew`安装那些依赖包。

8. 使用`brew cask`安装macOS应用程序、字体和插件以及其他非开源软件。

   ```shell
   $ brew cask install google-chrome
   ```

9. 制作一个cask跟创建一个配方（formula）一样简单

   ```shell
   $ brew cask create foo
   Editing /usr/local/Homebrew/Library/Taps/homebrew/homebrew-cask/Casks/foo.rb
   ```

### Homebrew基本使用

1. 安装包

   ```shell
   $ brew install <package_name>			
   ```

2. 卸载包

   ```shell
   $ brew uninstall <package_name>
   ```

3. 搜索包

   ```shell
   $ brew search <package_name>
   ```

   

4. 更新Homebrew在服务器上的包目录

   ```shell
   $ brew update
   ```

5. 查看你的包是否需要更新

   ```shell
   $ brew outdated
   ```

6. 更新指定包

   ```shell
   $ brew upgrade <package_naem>
   ```

   

7. Homebrew会将老版本的包缓存下来，以便当你想回滚至旧版本时使用。但这是比较少使用的情况，当你想清理旧版本包的缓存时，你可以运行

   ```shell
   $ brew cleanup
   ```

   

8. 查看你安装的包列表及版本号

   ```shell
   $ brew list --versions
   ```

9. 卸载Homebrew

   ```shell
   $ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/uninstall)"
   ```

   

### Homebrew Cask的安装及基本使用

1. 安装Homebrew-cask

   ```shell
   $ brew tap caskroom/cask // 添加 Github 上的 caskroom/cask 库
   $ brew install brew-cask  // 安装 brew-cask
   $ brew cask install google-chrome // 安装 Google 浏览器
   $ brew update && brew upgrade brew-cask && brew cleanup // 更新
   ```

   

2. 搜索Homebrew-cask软件信息

   ```shell
   $ brew cask info chrome
   ```

3. 安装Homebrew-cask软件

   ```shell
   $ brew cask install <package_name>
   ```

### 换源

1. 替换核心软件仓库

   ```shell
   cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
   git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git
   ```

   

2. 替换 cask 软件仓库（提供 macOS 应用和大型二进制文件）

   ```shell
   cd "$(brew --repo)"/Library/Taps/caskroom/homebrew-cask
   git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-cask.git
   ```

   

3. 替换 Bottles 源（Homebrew 预编译二进制软件包）

   bash用户：

   ```shell
   echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.bash_profile
   source ~/.bash_profile
   ```

   shell用户：

   ```shell
   
   echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.zshrc
   source ~/.zshrc
   ```

### Apps推荐

#### 设计工具

- Acorn - 一个像 PS，全面的功能集的图像编辑器。

- Affinity Designer - 矢量图像设计工具，可能的 Adobe Illustrator 的替代。

- Affinity Photo - 光栅图像设计工具，可以替代 Adobe PS 图象处理软件。

- Blender - 全功能可扩展的跨平台 3D 内容套件。

- Sculptris - 所见所得玩 3D 建模。

- Pixelmator - 强大的图像编辑器，可能 PS 图像处理软件的选择。

- Sketch - 混合矢量 / 位图布局应用，特别适用于用户界面，Web 和移动设计。

- - Sketch Toolbox - 一个超级简单的 Sketch 插件管理器。
  - Measure - 设计稿标注、测量工具。
  - User Flows - 直接从画板生成流程图。

- Sketch Cache Cleaner - 清理 Sketch 历史文件，释放磁盘空间。

- Gravit Designer - 混合矢量 / 位图布局应用，比起 Sketch 还差一点。

- Vectr - 免费图形编辑器。这是一个简单而强大的 Web 和桌面跨平台工具，把你的设计变成现实。

- Principle -  使用它很容易设计动画和交互式用户界面。

- Figma - 一款基于 Web 的实时协作的云设计软件。

#### 原型流程

- Axure RP 8 - 画原型图工具，团队协作 SVN 方便好用。
- ProtoPie - 高保真交互原型设计。
- Adobe XD (Experience Design) - 用于网站和移动应用的设计和原型设计。
- Balsamiq Mockups - 一个快速的网页设计原型工具，帮助你更快、更聪明的工作。
- Origami Studio - 一种设计现代界面的新工具，由 Facebook 设计师构建和使用。
- Flinto - 快速制作高保真的互交原型工具，支持 Sketch 导入。
- Kite - 一个强大的动画制作工具制作 Mac 和 iOS 原型中的应用。
- Justinmind - 功能更丰富团队协作方便。
- MockFlow - 用于网页设计和可用性测试的在线原型设计套件。
- pencil - 开源免费制作软件原型的工具  
- Mockplus - 更快更简单的原型设计工具。
- OmniGraffle - 可用来绘制图表、流程图、组织结构图、思维导图以及插图或原型。
- XMind - 一款实用的思维导图软件。
- Lighten - XMind 出品的一款实用的思维导图软件。
- Scapple - 一款实用的思维导图软件。
- Framer - 做交互原型的工具。
- Marvel - 简单设计，原型设计和协作。
- MindNode - 简洁的风格与人性化的操作，绘制思维脑图。
- WriteMapper - 专为写作者而设的脑图工具。
- SimpleMind - 超小体积的思维导图工具。
- macSVG - 设计 HTML5 SVG 和动画。

#### 其他实用工具

- 12306ForMac - Mac 版 12306 订票 / 捡票 助手。
- AirServer - 将手机投影到电脑上。
- CheatSheet - CheatSheet 是一款 Mac 上的非常实用的快捷键快速提醒工具。
- Snap - 一款可以给 Dock 上的程序添加快捷键的小工具
- iStat pro - 免费的 Mac OS 电脑硬件信息检测软件。
- ControlPlane - 自定义 Mac 情景模式。某些场景让 Mac 自动静音或是自动打开 Mail 客户端等等。
- BetterTouchTool - 代替默认的系统操作方式（组合键、修饰键、手势等）。
- NoSleep - 合上盖子不休眠，可根据是否连接电源单独设置。
- Qbserve - 观察你如何度过你的时间。
- Timing - Mac 的自动时间和生产力跟踪。
- RescueTime - 个人分析服务，向您展示如何花时间和提供工具来帮助您提高工作效率。
- HTTrack - 可以下载整个网站和离线浏览。
- OmniPlan - 项目管理软件。
- Focus - 一个漂亮的番茄工作法为基础的时间管理工具。
- OmniFocus - 由 OmniGroups 制作的 Nice GTD 应用程序。
- Taskade - 实时协作编辑器，协作简历任务管理器，大纲和笔记。