## 课程1 内存管理笔记

### 设计内存的分配与释放

#### 设计需求

- 在不同的设备中，都需要使用到资源申请/资源释放等问题，要求是在不同的设备下这些接口对外的参数都是一致的，这样一来方便上方的调用者使用统一的接口进行调用。
- 另外还需要对申请到的资源进行统一的管理，相当于申请到的是一块裸指针，但是常常会忘记释放它，那么如何进行有效的管理呢，可以通过智能指针记录根据它的使用者，并没有使用资源的自动释放。

首先将资源申请器抽象为一个父类，称为DeviceAllocator类，它具有一个allocate接口，负责申请该类设备下的size大小个字节。有一个release接口，负责回收对该设备申请得到的资源指针ptr.完整代码见include\base\alloc.h。

```C++
class DeviceAllocator {
 public:
  explicit DeviceAllocator(DeviceType device_type) : device_type_(device_type) {}

  virtual DeviceType device_type() const { return device_type_; }
  
  // 需要释放的内存
  virtual void release(void* ptr) const = 0;
    
 // 需要申请的内存/显存字节数
  virtual void* allocate(size_t byte_size) const = 0;
    
  virtual void memcpy(const void* src_ptr, void* dest_ptr, size_t byte_size,
                      MemcpyKind memcpy_kind = MemcpyKind::kMemcpyCPU2CPU, void* stream = nullptr,
                      bool need_sync = false) const;

  virtual void memset_zero(void* ptr, size_t byte_size, void* stream, bool need_sync = false);

 private:
  DeviceType device_type_ = DeviceType::kDeviceUnknown;
};
```

其中device_type表示该资源申请类所属的设备类型，因为DeviceAllocator是一个基类，所以此处的值是unknown，而在派生类中该变量将具体指明当前设备的类型。在拷贝时需要提供额外的参数，包括拷贝的方向，是否需要同步等待拷贝结束need_sync等。


#### CPU内存分配器的实现
```C++
class CPUDeviceAllocator : public DeviceAllocator {
 public:
  explicit CPUDeviceAllocator();

  void* allocate(size_t size) const override;

  void release(void* ptr) const override;

  void memcpy(const void* src_ptr, void* dest_ptr, size_t size) const override;
};
```
CPU上的内存资源分配无非是使用malloc，所以在allocate接口里封装的就是malloc方法，所以在release中封装的就是free方法，用于交还分配好的内存。

GPU上的内存分配器，GPUDeviceAllocator的allocate方法封装了N卡中的cudamalloc用于申请显存，两类设备类中实现了同一个父类的接口方法。CudaAllocator子类在代码中的source/base/alloc_cu.cpp.

```C++
void* CPUDeviceAllocator::allocate(size_t byte_size) const {
  if (!byte_size) {
    return nullptr;
  }

  void* data = malloc(byte_size);
  return data;
}

void* CUDADeviceAllocator::allocate(size_t byte_size) const {
  if (!byte_size) {
    return nullptr;
  }
  void* ptr = nullptr;
  cudaError_t err = cudaMalloc(&ptr, byte_size);
  CHECK_EQ(err, cudaSuccess);
  return ptr;
}
```

当使用内存分配器得到的结果还是以指针形式返回的，在之后的过程中便存在一定