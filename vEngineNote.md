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
