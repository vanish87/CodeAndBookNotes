1. Singletion  
Meyers Singleton  
```cpp
class Singleton {
public:
static Singleton& Instance() {
  static Singleton theSingleton;
  return theSingleton;
}
/* more (non-static) functions here */
private:
Singleton(); // ctor hidden
Singleton(Singleton const&); // copy ctor hidden
Singleton& operator=(Singleton const&); // assign op. hidden
~Singleton(); // dtor hidden
};
```
2. Good example of Script with C++  
https://gamedevcoder.wordpress.com/2013/05/09/binding-c-with-lua-squirrel-game-monkey-and-ocaml/
3. It has slightly different viewport for deferred lighting model. if rendering to half size of screen, only need to set a viewport to lighting pass(with drawing a full-screen qual). Keep in mind that they are rendering in **screen space**.
4. D3dDeivce->QueryInterface  
=> IDXGIDevice->GetParent  
=> IDXGIAdapter->GetParent  
=> IDXGIFactory1  
or IDXGIFactory1->QueryInterface  
=> IDXGIFactory2(DirectX 11.1 or later)  
5. All SetCamera should take a pointer of Camera But set camera parameter to its members.
6. Rasterization includes clipping vertices to the view frustum, performing a divide by z to provide perspective, mapping primitives to a 2D viewport, and determining how to invoke the pixel shader. While using a pixel shader is optional, the rasterizer stage always performs clipping, a perspective divide to transform the points into homogeneous space, and maps the vertices to the viewport.
7. Clip coordinates are in the homogenous form of < x, y, z, w >  
Vertices (x,y,z,w), coming into the rasterizer stage are assumed to be in homogeneous clip-space.  
Dividing x, y, and z by w accomplishes **Perspective Division**. The resulting coordinates are called normalized device coordinates.  
The transform that converts eye-space coordinates into clip-space coordinates is known as the **projection transform**

------
## 从零开始手敲次世代游戏引擎
###[Articles source](https://zhuanlan.zhihu.com/c_119702958)
##Outlines
### [2](https://zhuanlan.zhihu.com/p/28598462): 编译clang，设置工具链，cmake
### 3：Linux clang编译设置
### 4：模块划分:
    1。输入管理模块，用来获取用户输入

    2。策略模块，用来执行策略

    3。场景管理模块，用来管理场景和更新场景

    4。渲染模块，用来执行渲染和画面输出

    5。音频音效模块，用来管理声音，混音和播放

    6。网络通信模块，用来管理网络通信

    7。文件I/O模块，用来管理资源的加载和参数的保存回复

    8。内存管理模块，用来调度管理内存上的资源

    9。驱动模块，用来根据时间，事件等驱动其它模块

    10。辅助模块，用来执行调试，log输出等辅助功能

    11。应用程序模块，用来抽象处理配置文件，特定平台的通知，创建窗口等需要与特定平台对接的部分
### 5: IRuntimeModule定义，ApplicationInterface定义
### 6：GraphicsManager定义
### 7，8：Windows和Linux的窗口实现
### 9，10：Linux和windows的2D画图，COM技术的说明
### 11，12：D3D11的基本架构，OpenGL的架构
### 13：OpenGL4.0的使用
### 14：OpenGL4.0的代码说明
### 15：D3D12的实现，使用ComPtr，Fence的使用
### 16：D3D12生成Torus Mesh和对应的网格贴图，Depth Texture
### 17：游戏引擎的工作流程以及对应的模块划分
    1. 我们首先需要一个建立一个跨平台的模块，它能够在不同的操作系统+图形API环境当中，为我们创建这个基本的上下文。（可能是窗口，可能是全屏FrameBuffer，也可能是Off Screen Buffer）
    2. 然后，我们需要对平台的硬件能力进行查询和遍历，找到平台硬件（这里特指GPU）所能够支持的画布格式，并且将1所创建的上下文的FrameBuffer格式指定为这个格式，GPU才能够在上面作画。
    3. CPU使用平台所支持的图形API创建绘图所需要的各种Heap/Buffer/View，生成资源描述子（RootSignature或者Descriptor），将各种资源的元数据（Meta Data)填入描述子，并传递给GPU
    4. CPU根据场景描述信息进行顶点数据/索引/贴图/Shader等的加载，并将其展开在GPU所能看到的（也就是在描述子里面登记过的）Buffer当中
    帧循环开始
    5. CPU读取用户输入（在之前的文章当中还未涉及），并更新用户可操作场景物体的位置和状态
    6. CPU执行游戏逻辑（包括动画、AI），并更新对应物体的位置和状态
    7. CPU进行物体的裁剪，找出需要绘制的物体（可见的物体）
    8. CPU将可见物体的位置和状态翻译成为常量，并把常量上传到GPU可见的常量缓冲区
    9. CPU生成记录GPU绘图指令的Buffer （CommandList），并记录绘图指令
    10. CPU创建Fence，以及相关的Event，进行CPU和GPU之间的同步
    11. CPU提交记录了绘图指令的Buffer（CommandList），然后等待GPU完成绘制（通过观察Fence）
    12. CPU提交绘制结果，要求显示（Flip或者Present）
    13. 帧循环结束
### 18，19：内存管理的实现：block chain方法
### 20，21： 配置类实现，代码整合
### 22：使用ispc实现数学库：[对比文章](http://i-saint.hatenablog.com/entry/2015/05/26/212441) 对于Particle，ComputeShader速度会更快？
### 23：Vector和Matrix的实现，swizzle的实现，Test的CMakeFile
### 24: 场景管理的设计考虑
### 25：文件IO，同步异步读取
### 26：文件系统，读取bitmap
### 27：多种3D文件的介绍，以及选择OpenGEX的原因。场景物体的分类
### 28：SceneObject和SceneNode定义，GUID的使用，顶点数组和索引数组的实现
