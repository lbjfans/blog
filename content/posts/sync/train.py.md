---
share: true
title: train.py
tags:
  - sync
dir: posts/sync/
date: 2025-07-25T20:40:00+08:00
summary: train.py
---

# wandb

[Home – Weights & Biases](https://wandb.ai/home?product=models)

```
pip install wandb
wandb login  # 使用上面网址的key登陆
```


尝试：name改为自己的team name，project也是
```python
import random

import wandb

# Start a new wandb run to track this script.
run = wandb.init(
    # Set the wandb entity where your project will be logged (generally your team name).
    entity="my-awesome-team-name",
    # Set the wandb project where this run will be logged.
    project="my-awesome-project",
    # Track hyperparameters and run metadata.
    config={
        "learning_rate": 0.02,
        "architecture": "CNN",
        "dataset": "CIFAR-100",
        "epochs": 10,
    },
)

# Simulate training.
epochs = 10
offset = random.random() / 5
for epoch in range(2, epochs):
    acc = 1 - 2**-epoch - random.random() / epoch - offset
    loss = 2**-epoch + random.random() / epoch + offset

    # Log metrics to wandb.
    run.log({"acc": acc, "loss": loss})

# Finish the run and upload any remaining data.
run.finish()
```

