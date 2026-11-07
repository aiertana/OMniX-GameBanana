# OMniX-GameBanana

OMniX 的 **GameBanana 独立插件**：在 OMniX 功能专区提供 GameBanana 在线 Mod
商店（浏览 / 搜索 / 一键下载安装到角色目录）。

插件与 OMniX 主程序分离维护 —— 整个文件夹可随时删除、替换，主程序不受影响。

## 安装

1. 到 [Releases](https://github.com/aiertana/OMniX-GameBanana/releases) 下载最新 `OMniX-GameBanana-x.x.x.zip`
2. 解压，把里面的 `GameBananaStore` 文件夹放进 OMniX 安装目录下的
   `GamesData\Plugins\`（没有该目录就手动创建）
3. 重启 OMniX → 功能专区 → 出现绿色「已识别」的 GameBanana 卡片
4. 点卡片上的「浏览」打开商店窗口

```
OMniX（安装目录）
└── GamesData
    └── Plugins
        └── GameBananaStore      ← 解压到这里
            ├── plugin.json
            └── OmniX.Plugin.GameBanana.exe
```

## 更新

下载新版 zip，覆盖 `GamesData\Plugins\GameBananaStore` 里的同名文件即可。

## 版本对应

| 插件版本 | 宿主 OMniX | 说明 |
| --- | --- | --- |
| v1.1.0+ | **v1.4.6+** | 管道直开商店窗口（秒开） |
| v1.1.0+ | 1.4.5 等旧版 | 自动降级为 UIA 模拟点击，进功能专区打开商店 |

插件对宿主版本没有硬性要求：新旧宿主都能用，只是打开速度和方式不同。

## 工作原理

点「浏览」拉起的 `OmniX.Plugin.GameBanana.exe` 是一个轻量启动器，
按三级降级激活宿主里的商店窗口：

1. **命名管道** —— 通知已运行的新版宿主直接打开商店窗口；
2. **启动参数** —— 宿主没在跑时带 `--open-gamebanana` 拉起；
3. **UIA 自动化** —— 旧宿主（1.4.5 等）两者都不认识，
   等主窗口出现后模拟点击「功能专区 → 打开商店」。

商店 UI（浏览 / 下载 / 安装到角色目录）由 OMniX 宿主渲染，
插件只负责入口激活 —— 因此卸载插件不影响 OMniX 其它功能。

## 从源码构建

```powershell
dotnet build src\OmniX.Plugin.GameBanana\OmniX.Plugin.GameBanana.csproj -c Release
# 产物在 src\OmniX.Plugin.GameBanana\bin\Release\net9.0-windows\
# 连同根目录的 plugin.json 一起放进 GameBananaStore 文件夹即可
```

## 发布

push `v*` tag 自动出包并发布 Release（见
`.github/workflows/release.yml`）：

```bash
git tag v1.1.1
git push origin v1.1.1
```

## License

MIT
