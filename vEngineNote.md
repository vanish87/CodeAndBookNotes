1. Singletion  
Meyers Singleton  
```c++
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
