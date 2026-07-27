# DataPhraser Releases

DataPhraser 安装包分发仓库。**不含源码**——源码在私有仓库 `Quinn-Talor/dataphraser`，这个仓库只用来公开托管可下载的安装包。

## 目录约定

按版本号建目录，每个目录放一份 `manifest.json`，记录该版本各平台安装包的文件名、大小、SHA256 校验值，以及真正下载用的 GitHub Release 链接：

```
v0.1.0/
  manifest.json
v0.2.0/
  manifest.json
...
```

**真正的安装包文件不提交进这个仓库**——GitHub 对普通 git 提交的单个文件有 100MB 硬限制，dmg/exe 通常远超这个大小。安装包实际以 [GitHub Release 附件](https://github.com/Quinn-Talor/dataphraser-releases/releases) 的形式上传（附件不受 100MB 限制），`manifest.json` 里的 `releaseUrl` 和各平台条目下的 `downloadUrl` 指向对应的 Release 附件直链。

## manifest.json 结构

```json
{
  "version": "0.1.0",
  "publishedAt": "2026-07-27T00:00:00Z",
  "releaseUrl": "https://github.com/Quinn-Talor/dataphraser-releases/releases/tag/v0.1.0",
  "assets": [
    {
      "platform": "mac-arm64",
      "filename": "DataPhraser-0.1.0-arm64.dmg",
      "downloadUrl": "https://github.com/Quinn-Talor/dataphraser-releases/releases/download/v0.1.0/DataPhraser-0.1.0-arm64.dmg",
      "size": 352321536,
      "sha256": "..."
    }
  ]
}
```

## 发布新版本流程

1. 在主仓库（`dataphraser`）打包出安装文件（`npm run pack` / `npm run pack:win`）
2. 打 tag：`git tag v0.1.0 && git push origin v0.1.0`（此仓库内）
3. `gh release create v0.1.0 <安装包文件...> --repo Quinn-Talor/dataphraser-releases --title "DataPhraser 0.1.0"`
4. 建目录 `v0.1.0/manifest.json`，填入 Release 附件的直链、大小、SHA256（`shasum -a 256 <文件>` 获取）
5. 主仓库的 `docs/version.json` 同步更新 `latestVersion` / `minVersion`（如需强制升级）
