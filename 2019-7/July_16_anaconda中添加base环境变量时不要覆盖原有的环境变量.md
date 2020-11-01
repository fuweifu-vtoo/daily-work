##July 16

1. 现在anaconda在安装的时候会询问是否要把anaconda加入环境中，但是和以前不一样的是，现在自动加入环境的不是[export PATH="/home/vtoo/anaconda3/bin:$PATH"]()这句话，而是[echo ". /home/vtoo/anaconda3/etc/profile.d/conda.sh" >> ~/.bashrc]()这句话，这样做的好处是，不会覆盖原有的python版本，而需要使用anaconda的时候，需要用conda激活base环境。详情可以看我将[export PATH="/home/vtoo/anaconda3/bin:$PATH"]()加入.bashrc后运行conda，系统报错：

~~~python
CommandNotFoundError: Your shell has not been properly configured to use 'conda activate'.
If your shell is Bash or a Bourne variant, enable conda for the current user with

    $ echo ". /home/vtoo/anaconda3/etc/profile.d/conda.sh" >> ~/.bashrc

or, for all users, enable conda with

    $ sudo ln -s /home/vtoo/anaconda3/etc/profile.d/conda.sh /etc/profile.d/conda.sh

The options above will permanently enable the 'conda' command, but they do NOT
put conda's base (root) environment on PATH.  To do so, run

    $ conda activate

in your terminal, or to put the base environment on PATH permanently, run

    $ echo "conda activate" >> ~/.bashrc

Previous to conda 4.4, the recommended way to activate conda was to modify PATH in
your ~/.bashrc file.  You should manually remove the line that looks like

    export PATH="/home/vtoo/anaconda3/bin:$PATH"

^^^ The above line should NO LONGER be in your ~/.bashrc file! ^^^

~~~

2. 还有一点是，路径配置是放在～/.bashrc之中的。

