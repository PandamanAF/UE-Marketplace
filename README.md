# UE-Marketplace

PsychEngine 1.0.4 Mod Market —— 0 成本静态服务器（GitHub Pages 托管）。

游戏内 ModMarket 请求 `index.json` 获取 Mod 清单，按条目下载 zip、校验 SHA256、解压到 `mods/` 目录。

## 目录结构

```
UE-Marketplace/
├─ index.json        # 商店主清单（ModMarket 请求入口）
├─ thumbs/           # Mod 预览缩略图（png）
└─ mods/             # Mod 压缩包（演示阶段直链存放；正式流程建议改走 GitHub Release）
```

## index.json schema（schema=1）

```jsonc
{
  "schema": 1,
  "market": {
    "name": "PsychEngine Mod Market",
    "peVersion": "1.0.4",
    "repo": "https://github.com/PandamanAF/UE-Marketplace",
    "updatedAt": "YYYY-MM-DD"
  },
  "mods": [
    {
      "id": "mod-00001",          // 唯一 ID，后续 PR 流程自动分配
      "name": "Mod 名称",
      "author": "作者",
      "version": "1.0",
      "peVersion": "1.0.4",       // 只展示兼容 1.0.4 的 Mod
      "description": "描述",
      "thumb": "thumbs/mod-00001.png",
      "size": 19934,              // zip 字节数
      "sha256": "d8025db4...",    // zip SHA256，客户端下载后校验
      "download": {
        "type": "direct",          // direct=直链；github=GitHub API+Token（未来私有存储用）
        "url": "mods/mod-00001.zip"
      }
    }
  ]
}
```

## 投稿流程（当前手动阶段）

1. Mod 打包为 zip（内部结构二选一均可，客户端会处理嵌套文件夹）：
   - 根目录直接含 `pack.json`
   - 或套一层文件夹（如 `My-Mod/pack.json`，官方模板格式）
2. `pack.json` 建议补充 `author` / `version` / `peVersion: "1.0.4"`（PsychEngine 忽略未知字段，不影响运行；商店展示需要）
3. 计算 zip 的 SHA256（PowerShell：`Get-FileHash mod.zip -Algorithm SHA256`）
4. zip 放入 `mods/`，缩略图（建议 256×256 png）放入 `thumbs/`，编辑 `index.json` 新增条目
5. push 到 main，Pages 自动更新

## 本地开发测试

```powershell
# 在仓库根目录启动本地静态服务器
python -m http.server 8080 --directory .
# 浏览器/游戏端访问
http://localhost:8080/index.json
```

## 线上地址（GitHub Pages 启用后）

```
https://PandamanAF.github.io/UE-Marketplace/index.json
```
