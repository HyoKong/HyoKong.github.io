---
title: 'Transform.PyTorch'
date: 2020-06-06
permalink: /posts/2020/06/blog1/
tags:
  - transform
  - pytorch
---

- #### Transform Framework:
  - Framework:
  <img src="https://pic3.zhimg.com/80/v2-ac2aa9d3e537464c6bf00b8562d6d7ca_720w.jpg" width="50%" div_align=center> 
  - encoder-decoder framework
- #### Encoder:
  - multi-head self attention mechanism
  - position-wise feed-forward network (fully-connected layer)
- #### Decoder:
  - multi-head self attention mechanism
  - multi-head context-attention mechanism
  - position-wise feed-forward network
- #### Attention:
  - Weighted + Avg.
  - Additive Attention v.s. Multiplicative Attention
- #### Self-Attention:
  - calculate attention score by oneself
- #### Context-Attention:
  - additive attention
  - local-base
  - general
  - dot-product
  - scaled dot-product
- #### Scaled Dot-Product Attention:
  - <img src="https://pic3.zhimg.com/80/v2-e2bc3a470b359e8bdce750843140897e_720w.jpg" width="50%" div_align=center> 
   ```python
    import torch
    import torch.nn as nn
    import torch.functional as F
    import numpy as np

    class ScaledDotProductAttention(nn.Module):
        """Scaled dot-product attention mechanism."""

        def __init__(self, attention_dropout=0.0):
            super(ScaledDotProductAttention, self).__init__()
            self.dropout = nn.Dropout(attention_dropout)
            self.softmax = nn.Softmax(dim=2)

        def forward(self, q, k, v, scale=None, attn_mask=None):
            """
            前向传播.
            Args:
                q: Queries张量，形状为[B, L_q, D_q]
                k: Keys张量，形状为[B, L_k, D_k]
                v: Values张量，形状为[B, L_v, D_v]，一般来说就是k
                scale: 缩放因子，一个浮点标量
                attn_mask: Masking张量，形状为[B, L_q, L_k]

            Returns:
                上下文张量和attention张量
            """
            attention = torch.bmm(q, k.transpose(1, 2))
            if scale:
                attention = attention * scale
            if attn_mask:
                # 给需要 mask 的地方设置一个负无穷
                attention = attention.masked_fill_(attn_mask, -np.inf)
        # 计算softmax
            attention = self.softmax(attention)
        # 添加dropout
            attention = self.dropout(attention)
        # 和V做点积
            context = torch.bmm(attention, v)
            return context, attention
        ```
- #### Multi-Head Attention:
 - <img src="https://pic4.zhimg.com/80/v2-b8b4befd9b96318047ffb3251421cbe3_720w.jpg" width="50%" div_align=center>
 ```python
 class MultiHeadAttention(nn.Module):

    def __init__(self, model_dim=512, num_heads=8, dropout=0.0):
        super(MultiHeadAttention, self).__init__()

        self.dim_per_head = model_dim // num_heads
        self.num_heads = num_heads
        self.linear_k = nn.Linear(model_dim, self.dim_per_head * num_heads)
        self.linear_v = nn.Linear(model_dim, self.dim_per_head * num_heads)
        self.linear_q = nn.Linear(model_dim, self.dim_per_head * num_heads)

        self.dot_product_attention = ScaledDotProductAttention(dropout)
        self.linear_final = nn.Linear(model_dim, model_dim)
        self.dropout = nn.Dropout(dropout)
	
        # multi-head attention之后需要做layer norm
        self.layer_norm = nn.LayerNorm(model_dim)

    def forward(self, key, value, query, attn_mask=None):
	# 残差连接
        residual = query
        dim_per_head = self.dim_per_head
        num_heads = self.num_heads
        batch_size = key.size(0)

        # linear projection
        key = self.linear_k(key)
        value = self.linear_v(value)
        query = self.linear_q(query)

        # split by heads
        key = key.view(batch_size * num_heads, -1, dim_per_head)
        value = value.view(batch_size * num_heads, -1, dim_per_head)
        query = query.view(batch_size * num_heads, -1, dim_per_head)

        if attn_mask:
            attn_mask = attn_mask.repeat(num_heads, 1, 1)

        # scaled dot product attention
        scale = (key.size(-1)) ** -0.5
        context, attention = self.dot_product_attention(
          query, key, value, scale, attn_mask)

        # concat heads
        context = context.view(batch_size, -1, dim_per_head * num_heads)

        # final linear projection
        output = self.linear_final(context)

        # dropout
        output = self.dropout(output)

        # add residual and norm layer
        output = self.layer_norm(residual + output)

        return output, attention
 ```
 - #### Mask:
    - Padding Mask:
    Because of the different length of different input sequences, we need to make zero-padding for those sequences which are short. Such zeros are meaningless for the attention mechanism.
    ```python
    def padding_mask(seq_k, seq_q):
        # seq_k 和 seq_q 的形状都是 [B,L]
        len_q = seq_q.size(1)
        # `PAD` is 0
        pad_mask = seq_k.eq(0)
        pad_mask = pad_mask.unsqueeze(1).expand(-1, len_q, -1)  # shape [B, L_q, L_k]
        return pad_mask
    ```
    - Sequence Mask:
    ```python
    def sequence_mask(seq):
        batch_size, seq_len = seq.size()
        mask = torch.triu(torch.ones((seq_len, seq_len), dtype=torch.uint8),
                        diagonal=1)
        mask = mask.unsqueeze(0).expand(batch_size, -1, -1)  # [B, L, L]
        return mask
    ```
- #### Positional Embedding:
    -   
