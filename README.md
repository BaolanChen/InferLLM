<div align="center">
  
KuiperLLM
===========================
<h4>A high-performance deep learning inference framework for large language model (LLM)</h4>

---
<div align="left">

### 项目内容
1. 采用最新的C++ 20标准去写代码，统一、美观的代码风格，良好的错误处理；
2. 优秀的项目管理形式，同时采用CMake+Git的方式管理项目；
3. 设计一个现代C++项目，采用单元测试和Benchmark去测试验证项目
4. 基于CPU算子和CUDA双后端实现，对时新的大模型（LLama3和Qwen系列）有非常好的支持。


### 项目大纲

| 课程节数                                              | 推理框架知识点  | 
| ----------------------------------------------------- |-----| 
| 1. 内存的管理和设备类                           | 1.设备类管理硬件相关的操作，包括初始化硬件，管理硬件属性等，包括显存申请方法等 2.Buffer类封装内存管理，进行自动化内存生命周期管理。3.统一CPU主存和GPU显存接口（申请、释放、拷贝）。 |
| 2. 算子类的设计                            | 1.如何在大型模型中设计算子层 2.如何在算子层中存储权重、输入及输出 3.如何设计算子调用计算过程的标准化接口 |
| 3. 张量的设计与实现                          | 1.怎么设计一个接口良好的多维矩阵类，接口包括读取，访问，写入等；2.第2节中的内存管理类如何进行联动，使得张量底层的显存/内存资源能被自动管理 3.如何设计工具方法(function)来方便对张量实例中数据的访问。 |
| 4.RMSNorm算子的Cuda实现          | 1. Cuda基础，Thread和Block的定义；2. RMSNorm算子的公式讲解；3.RMSNorm算子的CUDA和CPU实现；4.CUDA中的块内规约实现原理(BlockReduce)。 |


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
   
   请参考本项目配套课程，课程参加方式请看本文开头。


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
