---
share: true
title: jump
tags:
  - sync
dir: posts/sync/
date: 2025-07-25T20:40:00+08:00
summary: 跳板机
---

# 连接


parsec：本机连接到跳板机（多人使用，然后各自通过SSH连接到服务器）

vscode连接：`litian@10.xxx, pwd: litian`

# 环境

虚拟环境：系统python环境不可以改变，创建自己的虚拟环境，还需要每个项目一个环境
```py
# 使用 --without-pip 参数忽略 pip 安装, 第二个myvenv可以写自己的名字
python3 -m venv myvenv --without-pip

# 进入虚拟环境目录
cd myvenv

# 下载 get-pip.py
wget https://bootstrap.pypa.io/get-pip.py

# 在虚拟环境中运行安装脚本（关键步骤！）
# 使用虚拟环境内的 Python 解释器，指定用户目录为虚拟环境的 site-packages
./bin/python get-pip.py --prefix=.

# 激活环境
source bin/activate

# 检查 pip 是否可用
pip --version  # 应显示虚拟环境内的 pip 路径

# 后续自己的环境操作
./bin/pip install transformers
or
source 后直接使用pip

# 自己其他文件夹使用这个环境
../litian_venv/bin/python3 ./first.py 

# 退出环境
deactivate

# 删除 venv
rm -rf myenv  # Linux/Mac

```

导出依赖，方便其他人使用
- [Python基础：生成requirements.txt文件 - 知乎](https://zhuanlan.zhihu.com/p/687462277)
- `pip freeze > requirements.txt`
- `pipreqs ./ --encoding=utf8  --force`：只把使用的放入txt中
- `pip install -r requirements.txt`


vscode使用虚拟环境
- [mac使用python虚拟环境vscode_mob64ca12daebd0的技术博客_51CTO博客](https://blog.51cto.com/u_16213343/13195547)
![](/blog/images/Pasted%20image%2020250326015715.png)
```
./litian/myvenv
```

安装pytorch
- `你的代码 → PyTorch API → CUDA 加速层 → NVIDIA 驱动 → GPU硬件`
- 版本：`nvidia-smi查看`，如cuda12.2，通过`pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121`
- 官网查看：[Start Locally | PyTorch](https://pytorch.org/get-started/locally/)
- 检查是否安装成功
```
import torch

print(torch.__version__)          # 查看 PyTorch 版本
print(torch.cuda.is_available())  # 输出应为 True（表示 GPU 可用）
print(torch.cuda.get_device_name(0))  # 显示 GPU 型号（如 NVIDIA A100）
```


计算能力
- 报错：`FP8 quantized models is only supported on GPUs with compute capability >= 9.0 (e.g H100)`
- [CUDA GPUs - Compute Capability | NVIDIA Developer](https://developer.nvidia.com/cuda-gpus#)




# q

需要docker管理环境吗？

