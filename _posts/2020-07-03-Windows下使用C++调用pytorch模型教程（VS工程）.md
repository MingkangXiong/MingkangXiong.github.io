---
layout: post
title: Windows下使用C++调用pytorch模型教程（VS工程）
key: 20200703
tags: pytorch
  windows
  vs
---

如果读者有在VS下配置opencv工程的经历，那么在本教程的指导下配置起来将非常轻松。大致可以分为三步：
1. 生成Torch Script
2. 下载libtorch
3. 在VS下配置项目属性及系统环境变量

## 生成Torch Script
新建一个python脚本，写下
```py
import torch
import torchvision

model = torch.load('模型路径')

model.cuda()
# model.cpu()

# An example input you would normally provide to your model's forward() method.

example = torch.rand(1, 3, 224, 224).cuda()
# example = torch.rand(1, 3, 224, 224)

# Use torch.jit.trace to generate a torch.jit.ScriptModule via tracing.

traced_script_module = torch.jit.trace(model, example)
traced_script_module.save('model-gpu.pt')
# traced_script_module.save("model-cpu.pt")
```
注意该脚本的目录下要放入自己网络结构的代码。

## 下载libtorch
想要下载对应版本的libtorch可以参看[该文章](https://blog.csdn.net/qq_37569355/article/details/104246813)。Windows用以下链接。
```
https://download.pytorch.org/libtorch/cpu/libtorch-win-shared-with-deps-1.0.1.zip
https://download.pytorch.org/libtorch/cu80/libtorch-win-shared-with-deps-1.0.1.zip
https://download.pytorch.org/libtorch/cu90/libtorch-win-shared-with-deps-1.0.1.zip
https://download.pytorch.org/libtorch/cu100/libtorch-win-shared-with-deps-1.0.1.zip
```
修改版本号可以下载相应版本。下载后解压，并记住解压后文件夹路径，后续配置将用上该路径。

## 配置Visual Studio工程及系统环境
笔者使用的是win10和vs2015环境。
1. 新建一个项目工程，打开VS的“项目->属性”，在C/C++一栏的“附加包含目录”中添加libtorch的头文件路径
```
你自己的路径\libtorch-win-shared-with-deps-1.0.1\libtorch\include
```
2. 在“链接器->常规->附加库目录”中添加libtorch的静态库路径
```
你自己的路径\libtorch-win-shared-with-deps-1.0.1\libtorch\lib
```
3. 在“链接器->输入->附加依赖项”中写入静态库名称
```
c10_cuda.lib
c10.lib
caffe2_detectron_ops_gpu.lib
caffe2_gpu.lib
caffe2.lib
caffe2_module_test_dynamic.lib
clog.lib
cpuinfo.lib
libprotobuf.lib
libprotobuf-lite.lib
libprotoc.lib
onnxifi_dummy.lib
onnxifi_loader.lib
onnx.lib
onnx_proto.lib
torch.lib
```
4. 右键“此电脑”，点击“高级系统设置”，点击“环境变量”，在上下两个框中找到“Path”变量，添加libtorch的动态库路径（静态库和动态库在同一个文件夹下）
```
你自己的路径\libtorch-win-shared-with-deps-1.0.1\libtorch\lib
```
至此，环境配置结束，示例代码如下

```cpp
#include <torch/script.h>
#include <iostream>

int main(void)
{
	std::shared_ptr<torch::jit::script::Module> module = torch::jit::load("model-gpu.pt");
	
	assert(module != nullptr);

	std::cout << "Model is loaded!" << std::endl;
	// Create a vector of inputs.
	std::vector<torch::jit::IValue> inputs;
	inputs.push_back(torch::ones({ 1, 3, 224, 224 }).cuda());

	// Execute the model and turn its output into a tensor.
	at::Tensor output = module->forward(inputs).toTensor();

	std::cout << output << std::endl;

	return 0;
}
```

本文参看了以下链接：
[https://zhuanlan.zhihu.com/p/52806730](https://zhuanlan.zhihu.com/p/52806730)

[https://blog.csdn.net/qq_37569355/article/details/104246813](https://blog.csdn.net/qq_37569355/article/details/104246813)
