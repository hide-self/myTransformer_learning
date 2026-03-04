# Transformer算法原理

## 算法背景

Transformer 由Google的研究团队提出；论文发表于2017年。主要致力于在序列建模中提升并行性与长距离依赖建模能力，摆脱对循环与卷积的依赖。Transformer主要做的工作如下：

**结构**：利用编码器与解码器的结构

**三种注意力**：`编码器多头自注意力`、`交叉注意力`、`解码器多头自注意力（含因果掩码）`

**位置信息**：用位置编码赋予词向量序列信息

**残差连接 + 层归一化 + 前馈网络(FFN)**：形成标准层块，稳定深层训练。



影响与演进：

NLP 基石：BERT、大模型系列；

跨模态扩展：ViT（图像）、多模态大模型。



## 整体架构

![77262416253](img/1772624162537.png)



**编码器与解码器的复用**

编码器(Encoder)与解码器(Decoder)后面都有"xN"，这表示这两部分的内容都可以进行复用(nn.Sequential)





**模型大致运行机制举例**

Inputs同时输入三个词`I love you`，此时Outputs shifted right输入一个开始信号设为`"Start"`，Outputs接收到后输出一个`我`

之后Outputs shifted right输入一个`"Start" 我`，Outputs接收到后输出一个`爱`

之后Outputs shifted right输入一个`"Start" 我 爱`，Outputs接收到后输出一个`你`

之后Outputs shifted right输入一个`"Start" 我 爱 你`，Outputs接收到后输出一个结束信号`"End"`

之后Outputs shifted right输入一个`"Start" 我 爱 你 "ENd"`，此时接收到结束信号，模型结束



以上运行机制的**实例证明**就是：当前的大模型也都是一个字一个字地输出的。





**三个注意力机制的出现位置**

1.多头注意力机制

出现在Inputs之后的编码器

2.多头注意力机制（含因果掩码机制）

出现在Outputs shifted right之后的解码器

3.多头交叉注意力机制

出现在Outputs shifted right之后的解码器，但是同时接收编码器与解码器两方的参数



# Transformer算法实战