# ShulkerRDK.RPTemplate
[English](README.md)

适用于资源包的 [ShulkerRDK](https://github.com/LiPolymer/ShulkerRDK) 项目模板

### 本模板包含:
- 一份 `.gitignore` 文件
- 必要的项目配置
- 基本的 Minecraft 资源包结构
- 一些 LevitateTask 脚本
- ShulkerRDK 二进制文件
  - **ShulkerRDK** 本体 `srdk` `srdk.exe`
  - **ShulkerRRT** 插件 `./shulker/extension/ShulkerRDK.RRT.dll`
  - **ResourceMagick** 插件 `./shulker/extension/ShulkerRDK.ResourceMagick.dll`

*（以上二进制文件版本皆为 [`B0.12`](https://github.com/LiPolymer/ShulkerRDK/releases/tag/B0.12)）*

*ShulkerRDK 和扩展均基于 [GPLv3](https://www.gnu.org/licenses/quick-guide-gplv3.zh-cn.html) 协议获得许可*

## 开始使用
#### 0. 获取这个仓库的内容,打开 ShulkerRDK
您可以直接下载,或克隆/分叉... *您怎么方便怎么来!*

下载完成后,您可以通过以下方式运行 ShulkerRDK 并进入交互模式
 - 双击 `srdk.exe` *( 仅限 Windows )*
 - 在模板根目录打开终端,运行 `./srdk` *( Windows 用户请使用 PowerShell )*

 在交互模式中,您可以执行一些指令来对您的项目进行半自动操作
### 最初的配置
#### 1. 修改项目名称
这个模板的默认项目名称为 `Template` , 您应该将其修改为您自己的项目名称

您可以通过在交互模式中执行以下指令来修改项目名称 (这里假定您要修改为"My Project")
```
proj chname "My Project"
```
> [!IMPORTANT]
> 如果您准备使用此模板开启一个新项目, 您可能还需要手动修改 `pack.mcmeta` 文件

> [!TIP]
> 您可能注意到 `My Project` 是被包裹在一对引号中间的
>
> 这和 ShulkerRDK 解析指令和 Levitate 语句 的方式有关
>
> 这里 `proj chname <name>` 指令读取第三个参数作为项目名称, 若去掉引号, 则会导致第三个参数仅为 `My`, 而"多余"的第四个参数 `Project` 则并不会被使用
>
> 这一点在使用中较为重要,还请您 ~~理解并记忆~~ 注意
>
> 当然,如果您的项目名称不包含空格,您可以不使用引号包裹, 例如
> ```
> proj chname MyProject
> ```
#### 准备好您的项目
好的,现在请把目光移到我们的 `src` 文件夹

在这个模板中 `src` 文件夹被定义为 `资源根`, 这里存放的将会是您的项目的基本内容

> [!TIP]
> ShulkerRDK 被设计来做的工作就是对 `资源根` *(在这里即为 `src` )* (或其副本) 内的文件进行一定处理, 所以这里的目录结构和您稍后将会得到的成品包体内的结构很相似

> [!TIP]
> 如果您此前没有进行过资源包开发, 我们建议您浏览[这篇指南](https://zh.minecraft.wiki/w/Tutorial:%E5%88%B6%E4%BD%9C%E8%B5%84%E6%BA%90%E5%8C%85), 以了解一些必要的技术细节

我们已经为您提前准备了一份最最基本的 Minecraft 资源包目录结构, 您可以直接使用

> [!NOTE]
> 如果您想开始一个新项目,您可以[跳过接下来的内容](#第一次构建)

**迁移到 ShulkerRDK**

> **我使用 `.psd` 作为贴图源文件**
>
> 提前恭喜!您可以直接享受到 ShulkerRDK 带来的重要体验提升之一: **自动转化**
>
> 更多细节稍后为您呈现, 现在请将您的所有源文件按照**输出的资源包的目录结构**放置于 `./src` 文件夹中(包括 `*.psd`, `*.json`, `*.mcmeta` 等)
> 

> **我使用 `.png` 作为贴图源文件**
>
> 请将您的所有源文件按照**输出的资源包的目录结构**放置于 `./src` 文件夹中(包括 `*.png`, `*.json`, `*.mcmeta` 等)
> 
> --- 
> 如果您想从 `.png` 文件切换至 `.psd` 文件, 您可以简单地执行以下命令
> ```
> png2psd ./src
> ```
> ShulkerRDK 将会扫描整个 `./src` 目录, 找出所有 `*.png` 文件并自动创建 `*.psd` 格式副本
>
> **鉴于 ShulkerRDK 尚不成熟, 请您逐个检查转化后的文件是否正常, 再删除对应的 `*.png` 文件*

#### 第一次构建
现在您可以使用以下命令来使用 ShulkerRDK 创建您项目
```
build
```
执行后, 检查 `build` 文件夹, 您应该可以看到您的项目已经被自动打包了

#### 配置开发环境
借助 ShulkerRRT, 您可以使用 ShulkerRDK 监测文件变化, 并自动更新, 自动在 Minecraft 内重载资源包

不过在开始之前, 我们需要继续完善项目的设置

要使用自动重载, 您需要准备一个适配您项目的 Minecraft 客户端, 并在客户端安装 Mod [ShulkerRRT](https://modrinth.com/project/DhnASaqg) *(如果您使用 (Neo)Forge, 可以使用 [Sinytra Connector](https://modrinth.com/project/u58R1TMW) 来加载这个Mod)*

> [!TIP]
> 如果可能, 我们推荐您一并安装 [Paxi](https://modrinth.com/project/CU0PAyzb), 它可以强制加载位于它配置中的资源包, 您无需担心正在开发的资源包被意外卸载

安装完成后, 请启动一次客户端并将其关闭, 打开Mod配置文件夹, 打开配置文件 `shulkerRRT.json`, 修改 `isShulkerRDKManaged` 项的值为 `true`

找到启动器的 `导出启动脚本` 功能 *(此处以 HMCL 为例, 选中目标版本, 从主页面依次点击 `版本管理 -> 管理 -> 生成启动脚本`)*, 导出为 `.ps1` (PowerShell 脚本格式) 到本仓库内容的 `./shulker/local/client.ps1`

完成后, 打开 `./shulker/tasks/settings.lvt`, 修改其中的 `X:\Path\To\Your\Game` 到您的游戏目录 *(请注意检查版本隔离情况)*

> [!NOTE]
> 如果您在前面的步骤中安装了 Paxi, 请将`^gameRoot^\resourcepacks\debugging` 修改为 `^gameRoot^\config\paxi\resourcepacks\debugging`

完成后, 您可以尝试使用以下命令启动客户端
```
dev
```

根据本模板预置的 `dev.lvt`, 这会同时启动文件监视器, 您可以尝试修改 `./src/` 文件夹内的文件

在本模板中, ShulkerRDK 不对删除动作和缓存文件 *(`.psd~`(Krita), `.tmp`(Photoshop))* 做出反应

并且如果 修改/新建的文件 或 重命名后的文件 为 `.psd` 格式, 将会对其进行格式转化和PBR抽取

> [!TIP]
> 上文末提到的`PBR抽取`, 是指 ShulkerRDK 自动从一个 `.psd` 文件中提取PBR贴图图层 (使图层名字以 `s_`, `n_` 开头来标记), 合并后独立导出为 `*_s.png`, `*_n.png`
> 
> 不过请注意, 这~~应该~~不会对提取出的图层应用任何高级的混合选项, 并且会无视图层隐藏
>
> 所以本模板**使用普通方式转化普通纹理**, 您如果要使用PBR抽取, 请在每次转化前 (最好为保存前) **确保PBR纹理图层为隐藏状态**

好了, 到现在, 您已经完成 ShulkerRDK 的配置了!

#### (可选)内置版本管理
如果您想使用 ShulkerRDK 内置的版本管理来管理您的项目, 下面是相关的指令:

展示版本号
```
verm show
```

设置版本号 (这里假定您要设为 `1.0.0`)
```
verm set 1.0.0
```

如果您要继续使用 `x.x.x` 格式的话,还可以使用下面的指令

步进修复版本 (`x.x.x.` -> `x.x.x+1`)
```
verm  sfix
```
步进小版本 (`x.x.x.` -> `x.x+1.x`)
```
verm sminor
```
步进大版本 (`x.x.x.` -> `x+1.x.x`)
```
verm smajor
```

#### 碎碎念
首先感谢您选择 ShulkerRDK! 🤗

这个小玩意花费了我不少精力, 希望你喜欢! 😋

由于这个玩意可自定义程度极高, 特性较多, 文档可能还需要一段时间, 于是先做了这个模板仓库, 让大家能先用上这些核心功能 😚

如果您在使用中遇到任何问题, 欢迎前往Discussion, 我们会在能力范围内尽可能帮助你!

再次感谢您的使用!