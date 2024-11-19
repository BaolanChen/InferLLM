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
| 1. 内存的管理和设备类                         | 1. 设备类管理硬件相关的操作，包括初始化硬件，管理硬件属性等，包括显存申请方法等 2. Buffer类封装内存管理，进行自动化内存生命周期管理。3. 统一CPU主存和GPU显存接口（申请、释放、拷贝）。 |
