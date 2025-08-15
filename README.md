# ShulkerRDK.RPTemplate
[中文](README_zh.md)

A project template for resource packs based on [ShulkerRDK](https://github.com/LiPolymer/ShulkerRDK)

### This template includes:
- A `.gitignore` file
- Necessary project configurations
- Basic Minecraft resource pack structure
- Some LevitateTask scripts
- ShulkerRDK binary files
  - **ShulkerRDK** core `srdk` `srdk.exe`
  - **ShulkerRRT** plugin `./shulker/extension/ShulkerRDK.RRT.dll`
  - **ResourceMagick** plugin `./shulker/extension/ShulkerRDK.ResourceMagick.dll`

*(All binary files are version [`B0.12`](https://github.com/LiPolymer/ShulkerRDK/releases/tag/B0.12))*

*ShulkerRDK and extensions was licensed under [GPLv3](https://www.gnu.org/licenses/quick-guide-gplv3.zh-cn.html)*

## Getting Started
#### 0. Obtain the repository contents and launch ShulkerRDK
You can download directly, or clone/fork... *whatever is convenient for you!*

After downloading, you can run ShulkerRDK and enter interactive mode by:
 - Double-clicking `srdk.exe` *(Windows only)*
 - Opening a terminal in the template root directory and running `./srdk` *(Windows users please use PowerShell)*

In interactive mode, you can execute commands to perform semi-automated operations on your project.

### Initial Configuration
#### 1. Modify the project name
The default project name for this template is `Template`, which you should change to your own project name.

You can modify the project name by executing the following command in interactive mode (assuming you want to change it to "My Project"):
```
proj chname "My Project"
```
> [!IMPORTANT]
> If you're starting a new project with this template, you may also need to manually modify the `pack.mcmeta` file.

> [!TIP]
> You may have noticed that `My Project` is enclosed in quotation marks.
> 
> This relates to how ShulkerRDK parses commands and Levitate statements.
> 
> The `proj chname <name>` command takes the third parameter as the project name. Without quotes, the third parameter would only be `My`, and the "extra" fourth parameter `Project` would not be used.
> 
> This is quite important in usage, so please pay attention.
> 
> Of course, if your project name doesn't contain spaces, you can omit the quotation marks, for example:
> ```
> proj chname MyProject
> ```

#### Prepare Your Project
Now, let's turn our attention to the `src` folder.

In this template, the `src` folder is defined as the `resource root`, which will contain the basic content of your project.

> [!TIP]
> ShulkerRDK is designed to process files within the `resource root` (in this case, `src`) or its copies, so the directory structure here is very similar to what you'll get in the final package.

> [!TIP]
> If you haven't done resource pack development before, we recommend browsing [this guide](https://zh.minecraft.wiki/w/Tutorial) to understand some necessary technical details.

We've already prepared the most basic Minecraft resource pack directory structure for you, which you can use directly.

> [!NOTE]
> If you're starting a new project, you can [skip the following content](#first-build).

**Migrating to ShulkerRDK**

> **I use `.psd` as my texture source files**
>
> Congratulations in advance! You can directly enjoy one of the important experience improvements brought by ShulkerRDK: **automatic conversion**.
> 
> More details will be presented later. For now, place all your source files in the `./src` folder according to the **directory structure of the output resource pack** (including `*.psd`, `*.json`, `*.mcmeta`, etc.).
> 

> **I use `.png` as my texture source files**
>
> Place all your source files in the `./src` folder according to the **directory structure of the output resource pack** (including `*.png`, `*.json`, `*.mcmeta`, etc.).
> 
> --- 
> If you want to switch from `.png` to `.psd` files, you can simply execute the following command:
> ```
> png2psd ./src
> ```
> ShulkerRDK will scan the entire `./src` directory, find all `*.png` files, and automatically create `*.psd` format copies.
>
> **Since ShulkerRDK is not yet mature, please check each converted file individually before deleting the corresponding `*.png` files.**

#### First Build
Now you can use the following command to create your project with ShulkerRDK:
```
build
```
After execution, check the `build` folder. You should see that your project has been automatically packaged.

#### Configuring the Development Environment
With ShulkerRRT, you can use ShulkerRDK to monitor file changes, automatically update, and automatically reload resource packs in Minecraft.

However, before we begin, we need to further完善 the project settings.

To use automatic reloading, you need to prepare a Minecraft client adapted to your project and install the Mod [ShulkerRRT](https://modrinth.com/project/DhnASaqg) in the client *(if you're using (Neo)Forge, you can use [Sinytra Connector](https://modrinth.com/project/u58R1TMW) to load this Mod)*

> [!TIP]
> If possible, we recommend also installing [Paxi](https://modrinth.com/project/CU0PAyzb), which can force-load resource packs from its configuration, so you don't have to worry about your development resource pack being accidentally unloaded.

After installation, launch the client once and close it. Open the mod configuration folder, open the configuration file `shulkerRRT.json`, and change the value of `isShulkerRDKManaged` to `true`.

Find the launcher's "Export Launch Script" feature *(using HMCL as an example here, select the target version, then click `Version Management -> Manage -> Generate Launch Script` from the main page)*, and export it as `.ps1` (PowerShell script format) to `./shulker/local/client.ps1` in this repository.

After completion, open `./shulker/tasks/settings.lvt` and modify `X:\Path\To\Your\Game` to your actual path. *(Please check version isolation settings.)*

> [!NOTE]
> If you installed Paxi in the previous step, change `^gameRoot^\resourcepacks\debugging` to `^gameRoot^\config\paxi\resourcepacks\debugging`

After completion, you can try launching the client with the following command:
```
dev
```

According to the pre-configured `dev.lvt` in this template, this will also start the file watcher. You can try modifying files within the `./src/` folder.

In this template, ShulkerRDK does not respond to delete actions and cache files *(`.psd~`(Krita), `.tmp`(Photoshop))*.

Additionally, if modified/newly created files or renamed files are in `.psd` format, it will perform format conversion and PBR extraction.

> [!TIP]
> The "PBR extraction" mentioned above refers to ShulkerRDK automatically extracting PBR texture layers from a `.psd` file (marked by layer names starting with `s_`, `n_`), merging them, and independently exporting as `*_s.png`, `*_n.png`.
> 
> However, please note that this ~~should~~ will not apply any advanced blending options to the extracted layers, and will ignore hidden layers.
>
> Therefore, this template **uses standard conversion for regular textures**. If you want to use PBR extraction, please **ensure PBR texture layers are hidden** before each conversion (preferably before saving).

Alright, now you've completed the ShulkerRDK configuration!

#### (Optional) Built-in Version Management
If you want to use ShulkerRDK's built-in version management to manage your project, here are the relevant commands:

Show version number
```
verm show
```

Set version number (assuming you want to set it to `1.0.0`)
```
verm set 1.0.0
```

If you continue to use the `x.x.x` format, you can also use the following commands:

Step up patch version (`x.x.x.` -> `x.x.x+1`)
```
verm sfix
```
Step up minor version (`x.x.x.` -> `x.x+1.x`)
```
verm sminor
```
Step up major version (`x.x.x.` -> `x+1.x.x`)
```
verm smajor
```

#### Random Thoughts
First of all, thank you for choosing ShulkerRDK! 🤗

This little tool has taken me quite some effort, hope you like it! 😋

Since this tool has extremely high customizability and many features, the documentation might still need some time. So I made this template repository first to let everyone use these core features 😚

If you encounter any problems during use, feel free to go to Discussions. We'll help you as much as we can within our capabilities!

Thank you again for your usage!