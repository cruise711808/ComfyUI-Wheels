# ComfyUI-Wheels

用于保存 ComfyUI/Linux 环境使用的预编译 Python wheel。目录按下面的维度组织：

```text
wheels/<package>/<upstream-version>/<build-variant>/<platform>/<python-abi>/
```

当前发布包：

| 包 | 上游版本 | 构建变体 | Python / ABI | 平台 | SHA256 |
| --- | --- | --- | --- | --- | --- |
| `sageattention` | `2.2.0` | `cu132torch2130` | `cp314-cp314` | `linux_x86_64` | `0e30552a5139db66c23d03e4fc293649234d9de171d3b9901399d13feb7eb0f6` |

## 安装

在 ComfyUI 的虚拟环境中执行：

```bash
python -m pip install \
  wheels/sageattention/2.2.0/cu132torch2130/linux_x86_64/cp314/\
  sageattention-2.2.0+cu132torch2130-cp314-cp314-linux_x86_64.whl
```

该 wheel 适用于 CPython 3.14、Linux x86_64，并针对 PyTorch `2.13.0+cu132` 构建。构建环境使用 CUDA Toolkit `13.3`，包含 `sm80` 和 `sm89` 内核。完整构建信息见对应目录的 `build-info.json`。

