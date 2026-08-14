# ComfyUI-Wheels

## 文件规范

- 目录固定为：`<CUDA与PyTorch组合>/<标准化包名>/<wheel 文件>`
- 例如：`cu132torch2.13/sageattention/<wheel 文件>`
- 只提交预编译的 `.whl` 文件，不提交 `build-info.json`、临时目录或构建缓存。
- 文件名使用标准 wheel 格式：
  `<包名>-<版本>-<Python标签>-<ABI标签>-<平台标签>.whl`
- CUDA 与 PyTorch 构建环境由索引目录区分，不写入 wheel 版本或文件名；包内
  `METADATA` 的版本必须与文件名中的版本保持一致。
- `.whl` 文件必须使用 Git LFS 管理。

## pip/uv 索引

本仓库为 CUDA 13.2、PyTorch 2.13 提供单层静态索引：

```text
https://cruise711808.github.io/ComfyUI-Wheels/cu132torch2.13/
```

安装两个预编译包：

```bash
uv pip install \
  --extra-index-url https://cruise711808.github.io/ComfyUI-Wheels/cu132torch2.13/ \
  --no-deps \
  flash-attn sageattention
```

索引根目录下的包名目录遵循 Python Simple Repository API 的名称规范；
`flash_attn` 在 URL 中标准化为 `flash-attn`。
