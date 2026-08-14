# ComfyUI-Wheels

## 文件规范

- 目录固定为：`<应用>/<平台>/<文件>`
- 例如：`sageattention/linux/<wheel 文件>`
- 只提交预编译的 `.whl` 文件，不提交 `build-info.json`、临时目录或构建缓存。
- 文件名必须使用标准 wheel 格式，并可在文件名中加入构建变体：
  `<包名>-<版本>+<构建变体>-<Python标签>-<ABI标签>-<平台标签>.whl`
- 只改文件名，不修改 wheel 内容；包内 `METADATA` 的版本必须保持上游版本。
- `.whl` 文件必须使用 Git LFS 管理。
