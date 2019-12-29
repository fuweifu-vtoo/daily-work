#July 5
------
##some about install cuda
- 以后配置一个新的ubuntu的时候，如果需要安装CUDA和nvidia驱动，要记住不要自己去装nvidia驱动，因为安装cuda的时候会自己安装一个驱动，这个驱动可能不是最新的，但是它是最适合当前CUDA版本的驱动。所以以后直接安装CUDA就好了。
- nvidia-smi 只能看一个瞬间的GPU的工作状态，而 ++watch -n 0.1 nvidia-smi++ 可以一直监控GPU，mark住。





---
##something about internet
- 能够在电脑上使用微信，但是不能用浏览器上网，一般是DNS解析的原因。因为上浏览器需要靠DNS解析网址ip，而用qq和wechat不需要这个。