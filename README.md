# ComfyUI-Wheels

## 文件规范

- 目录固定为：`<应用>/<平台>/<文件>`
- 例如：`sageattention/linux/<wheel 文件>`
- 只提交预编译的 `.whl` 文件，不提交 `build-info.json`、临时目录或构建缓存。
- 文件名必须使用标准 wheel 格式：
  `<包名>-<版本>-<Python标签>-<ABI标签>-<平台标签>.whl`
- CUDA/PyTorch 构建变体写入版本的本地标识，例如：`2.2.0+cu132torch2130`。
- `.whl` 文件必须使用 Git LFS 管理。
