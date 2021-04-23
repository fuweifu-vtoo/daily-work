## OHEM

OHEM的实现主要有两种办法：

a. An obvious way

直接修改损失层，然后直接进行hard example selection。损失层计算所 有的RoIs，然后按损失从大到小排序，当然这里有个NMS(非最大值抑制) 操作，选择hard RoIs并non-hard RoIs的损失置0。虽然这方法很直接，

但效率是低下的，不仅要为所有RoI分配内存，还要对所有RoI进行反向 传播，即使有些RoI损失为0。

```
作者：Peiyuan Liao
链接：https://www.zhihu.com/question/272988870/answer/562262315
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

def focal_loss(self, output, target, alpha, gamma, OHEM_percent):
        output = output.contiguous().view(-1)
        target = target.contiguous().view(-1)

        max_val = (-output).clamp(min=0)
        loss = output - output * target + max_val + ((-max_val).exp() + (-output - max_val).exp()).log()

        # This formula gives us the log sigmoid of 1-p if y is 0 and of p if y is 1
        invprobs = F.logsigmoid(-output * (target * 2 - 1))
        focal_loss = alpha * (invprobs * gamma).exp() * loss

        # Online Hard Example Mining: top x% losses (pixel-wise). Refer to http://www.robots.ox.ac.uk/~tvg/publications/2017/0026.pdf
        OHEM, _ = focal_loss.topk(k=int(OHEM_percent * [*focal_loss.shape][0]))
        return OHEM.mean()
```

b. A better way

省略