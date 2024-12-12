<div align="center">
  
KuiperLLM
===========================
<h4>A high-performance deep learning inference framework for large language model (LLM)</h4>
<h4>Implement KuiperLLM based on C++ from Scratch</h4>

---
<div align="left">

### 命名含义
Kuiper命名来源于“Project Kuiper”计划，其含义为希望开源研发人员像各个卫星一样组成网络进行协同，共同研发完成Kuiper项目。

“Project Kuiper”是亚马逊旗下太空互联网项目，计划在低地球轨道上建立一个由3236颗卫星组成的网络，为全球各地提供高速互联网。
2022年，亚马逊宣布，公司向法国阿丽亚娜太空（Arianespace）、美国联合发射联盟（ULA）以及蓝色起源三家企业预定了 83 次火箭发射，计划在五年时间内将数千颗卫星送入地球轨道，成为有史以来最大的发射服务商业采购。

2023年10月6日14时许，美国联合发射联盟公司用运载火箭为亚马逊首次发射两颗太空互联网原型卫星，标志着其卫星网络“柯伊伯计划（Project Kuiper）”的正式启动。亚马逊计划在未来6年建立一个由3236颗低轨道互联网卫星组成的网络，旨在提供“快速”、“经济”的互联网接入服务，亚马逊预计在2024年上半年发射首批量产卫星。

### 项目内容
1. 采用最新的C++ 20标准去写代码，统一、美观的代码风格，良好的错误处理；
2. 优秀的项目管理形式，同时采用CMake+Git的方式管理项目；
3. 设计一个现代C++项目，采用单元测试和Benchmark去测试验证项目
4. 基于CPU算子和CUDA双后端实现，对时新的大模型（LLama3和Qwen系列）有非常好的支持。


### 项目大纲

| 实现节数                                              | 推理框架知识点  | 
| ----------------------------------------------------- |-----| 
| 1. 内存的管理和设备类                           | 1.设备类管理硬件相关的操作，包括初始化硬件，管理硬件属性等，包括显存申请方法等 2.Buffer类封装内存管理，进行自动化内存生命周期管理。3.统一CPU主存和GPU显存接口（申请、释放、拷贝）。 |
| 2. 算子类的设计                            | 1.如何在大型模型中设计算子层 2.如何在算子层中存储权重、输入及输出 3.如何设计算子调用计算过程的标准化接口 |
| 3. 张量的设计与实现                          | 1.怎么设计一个接口良好的多维矩阵类，接口包括读取，访问，写入等；2.第2节中的内存管理类如何进行联动，使得张量底层的显存/内存资源能被自动管理 3.如何设计工具方法(function)来方便对张量实例中数据的访问。 |
| 4.RMSNorm算子的Cuda实现          | 1. Cuda基础，Thread和Block的定义；2. RMSNorm算子的公式讲解；3.RMSNorm算子的CUDA和CPU实现；4.CUDA中的块内规约实现原理(BlockReduce)。 |
| 5.Nsight调优Cuda算子          | 1. 让更多的Cuda线程来参与归约计算的过程；2. 纠正第5次课程cuda代码中的一些纰漏；3. 利用Nsight软件的分析结果指导Cuda代码的编写和调优。 |
| 6.LLama模型的量化          | 1. 怎么导出量化后的LLama模型；2. 加载LLama量化模型和非量化模型有什么异同；3. 怎么利用量化系数反推得到量化前的浮点权重。｜

### 第三方依赖
> 借助企业级开发库，更快地搭建出大模型推理框架
1. google glog https://github.com/google/glog
2. google gtest https://github.com/google/googletest
3. sentencepiece https://github.com/google/sentencepiece
4. armadillo + openblas https://arma.sourceforge.net/download.html
5. Cuda Toolkit


### 模型下载地址
1. LLama2 https://pan.baidu.com/s/1PF5KqvIvNFR8yDIY1HmTYA?pwd=ma8r 或 https://huggingface.co/fushenshen/lession_model/tree/main

2. Tiny LLama 
- TinyLLama模型 https://huggingface.co/karpathy/tinyllamas/tree/main
- TinyLLama分词器 https://huggingface.co/yahma/llama-7b-hf/blob/main/tokenizer.model

3. Qwen2.5/LLama

### 模型导出
```shell
python export.py llama2_7b.bin --meta-llama path/to/llama/model/7B
# 使用--hf标签从hugging face中加载模型， 指定--version3可以导出量化模型
# 其他使用方法请看export.py中的命令行参数实例
```
### 编译方法
```shell
  mkdir build 
  cd build
  # 需要安装上述的第三方依赖
  cmake ..
  # 或者开启 USE_CPM 选项，自动下载第三方依赖
  cmake -DUSE_CPM=ON ..
  make -j16
```
