# Facebook最新开源工具pytext实战

作者：吕青松

## pytext是什么

Facebook开源的工具一直以简单易用闻名，比如fastText<sup>[1]</sup>，一行命令即可训练词向量或者训练一个文本分类器。就在2018年12月，Facebook又开源了一个强大的NLP工具：pytext。它可以把NLP问题流水线化，使得学术模型到产业应用无缝衔接。<sup>[2]</sup>说得直白一些，工程师再也不用苦逼地看各种paper的代码了！给定参数文档，调用三个命令：训练、测试、预测，一个模型就这样从学术界走向产业界！

## pytext实践

这篇文章就以文本分类任务为例演示一下pytext的使用方法。具体任务：给定标题，预测属于自然科学基金8个学科门类里的哪一类。<sup>[3]</sup>，训练数据来源：科学基金共享服务网<sup>[4]</sup>，20万项目标题和对应的学科标签。训练集、验证集、测试集划分比例：7:1:2。

运行环境：Ubuntu 18.04，python3.6，virtualenv，个人PC，无GPU。

首先，下载相关代码：

```bash
git clone https://github.com/1049451037/pytext_clf
cd pytext_clf
```

然后，配置虚拟环境：

```bash
python3 -m venv pytext_venv
source pytext_venv/bin/activate
pip install wheel
pip install pytext-nlp
pip install jieba
```

训练：

```bash
pytext train < config.json
```

这个过程大概要一顿午饭的时间，因为我上传了我训练好的模型，如果不想等，可以直接跳到使用模型部分。

这里使用的config.json是pytext的demo里提供的<sup>[5]</sup>配置，使用双向LSTM+Attention模型，未调参。

测试：

```bash
pytext test < config.json
```

准确率78.40%。

保存模型：

```bash
pytext export --output-path exported_model.c2 < config.json
```

使用模型：

```bash
python run_model.py
```

用完退出虚拟环境：

```bash
deactivate
```

以上就是pytext使用的例子，未经任何调参就达到了78.40%的分类准确率。对于上述过程用fastText做了一次，用验证集仔细调参，最好的效果是77.68%，所以pytext还是很给力的。


## 参考资料

[1] https://github.com/facebookresearch/fastText

[2] https://arxiv.org/abs/1812.08729

[3] http://www.nsfc.gov.cn/publish/portal0/tab226/

[4] http://npd.nsfc.gov.cn/protype.action

[5] https://github.com/facebookresearch/pytext/blob/master/demo/configs/sst2.json
