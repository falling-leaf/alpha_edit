## 配置注意事项

1. 论文中给出的README.md中的Pytorch和transformers版本不够，用下面的方法安装才能跑llama 3:

```bash
pip install torch==2.1.0 torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118 && \
pip install transformers==4.38.0 einops==0.4.0 higher==0.2.1 hydra-core==1.2.0 datasets==2.15.0 \
matplotlib==3.6.1 spacy==3.4.1 scipy==1.9.2 scikit-learn==1.0.2 nltk==3.7
```

2. 实际启动命令中，如果直接用huggingface.co的模型需要认证（试了好几次不同国家和账号都被拒绝了），所以这边改用了modelscope的模型，因此启动语句改为：

```bash
python3 -m experiments.evaluate     --alg_name=AlphaEdit     --model_name=/home/jjsu/.cache/modelscope/hub/models/LLM-Research/Meta-Llama-3-8B-Instruct     --hparams_fname=Llama3-8B.json --ds_name=mcf --dataset_size_limit=2000    --num_edits=100 --downstream_eval_steps=5
```

> 需要提前下载好对应的模型

3. wikipedia数据集过期（但如果提前下好了P矩阵应该不会有问题，要改成20220301的版本）

4. P的reload问题：原代码中似乎没有给到如果使用缓存的情况，那么这里重写了一个入口（在250行左右）

5. 命名问题：似乎是modelscope和论文代码命名不一致，将useful_functions.py中的llama3名称进行修改：

llama3-8b-instruct ---> meta-llama-3-8b-instruct