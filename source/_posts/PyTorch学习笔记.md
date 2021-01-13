---
title: PyTorch学习笔记
date: 2021:1:12 21:54
categories: 学习笔记
tags: 
- PyTorch
- 笔记
---

## repeat()和expand() 区别
torch.Tensor()是包含一种数据类型元素的多维矩阵。
torch.Tensor()有两种方法可以用来拓展某维数据的尺寸，分别是expand()和repeat():

### expand()
返回当前张量在某维扩展后更大的张量。扩展张量不会分配新的内存，只是在当前存在的张量上创建新的视图，一个大小等于1的维度扩展到更大的尺寸。
### repeat()
