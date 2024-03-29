---
tags:
- encryption 
alias:
- 哈夫曼编码
---

哈夫曼编码（Huffman coding）是一种常见的变长编码技术，用于无损数据压缩。它是由David A. Huffman于1952年发明的，并且被广泛应用于数据传输和存储领域，尤其在图像、音频和视频压缩中被广泛使用。

哈夫曼编码的核心思想是根据数据的出现频率来分配不同长度的编码，使得高频率的数据用短编码表示，低频率的数据用长编码表示，从而达到压缩数据的目的。

## 基本步骤
1. 统计频率：首先，需要对待压缩的数据进行扫描，统计每个数据符号（通常是字节或字符）在数据中出现的频率。
    
2. 构建哈夫曼树：根据频率信息构建哈夫曼树，哈夫曼树是一种特殊的二叉树，它的叶子节点对应数据符号，并且每个叶子节点的权重为其对应数据符号的频率。构建哈夫曼树的过程是通过反复合并两个权重最小的节点，直到只剩下一个根节点为止。
    
3. 分配编码：通过从根节点出发，沿着左子树路径走为0，沿着右子树路径走为1，将每个叶子节点的编码逐步确定下来。由于哈夫曼树的构建过程中，频率较高的符号会距离根节点较近，因此它们的编码会较短，频率较低的符号编码会较长。
    
4. 压缩数据：用生成的哈夫曼编码替换原始数据中的符号，从而将原始数据压缩为更短的编码表示。

