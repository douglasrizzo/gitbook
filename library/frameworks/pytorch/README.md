---
description: How to integrate a PyTorch script to log metrics to W&B
---

# PyTorch

W&B provides first class support for PyTorch. To automatically log gradients and store the network topology, you can call `watch` and pass in your PyTorch model.

```python
import wandb
wandb.init(config=args)

# Magic
wandb.watch(model)

model.train()
for batch_idx, (data, target) in enumerate(train_loader):
    output = model(data)
    loss = F.nll_loss(output, target)
    loss.backward()
    optimizer.step()
    if batch_idx % args.log_interval == 0:
        wandb.log({"loss": loss})
```

> Gradients, metrics and the graph won't be logged until `wandb.log` is called after a forward and backward pass.

See this [Colab notebook](https://colab.research.google.com/github/wandb/examples/blob/master/pytorch-intro/intro.ipynb) for an end to end example of integrating wandb with PyTorch. You can also find more examples in our [example projects](../../example-projects/) section.

### Options

By default the hook only logs gradients.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Arguments</th>
      <th style="text-align:left">Options</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">log</td>
      <td style="text-align:left">
        <ul>
          <li>all: log histograms of gradients and parameters</li>
          <li>gradients (default)</li>
          <li>parameters</li>
          <li>None</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">log_freq</td>
      <td style="text-align:left">integer (default 100): The number of steps between logging gradients</td>
    </tr>
  </tbody>
</table>

## Images

You can pass PyTorch tensors with image data into `wandb.Image` and torchvision utils will be used to log them automatically.

To log images and view them in the Media panel, you can use the following syntax:

```python
wandb.log({"examples" : [wandb.Image(i) for i in images]})
```

## Multiple Models

If you need to track multiple models in the same script, you can wall wandb.watch\(\) on each model separately.

