## Command pattern
* handle input command
``` cpp
Command* command = inputHandler.handleInput();
if (command)
{
 command->execute(actor);
}
```
## Flyweight
* share resource between objects
``` cpp
private:
 Terrain grassTerrain_;
 Terrain hillTerrain_;
 Terrain riverTerrain_;

 class World
{
private:
 Terrain* tiles_[WIDTH][HEIGHT];
 // Other stuff...
};
```

## Observer
* To unlock an achievement
``` cpp
class Observer
{
public:
 virtual ~Observer() {}
 virtual void onNotify(const Entity& entity,
 Event event) = 0;
};

class Subject
{
public:
 void addObserver(Observer* observer)
 {
 // Add to array...
 }
 void removeObserver(Observer* observer)
 {
 // Remove from array...
 }
protected:
void notify(const Entity& entity, Event event)
{
  for (int i = 0; i < numObservers_; i++)
  {
    observers_[i]->onNotify(entity, event);
  }
};
```

## Prototype
* To create objects
``` cpp
class Spawner
{
public:
 virtual ~Spawner() {}
 virtual Monster* spawnMonster() = 0;
};
template <class T>
class SpawnerFor : public Spawner
{
public:
 virtual Monster* spawnMonster() { return new T(); }
};
```

## State
* FSM for different situation handling
* State object should not store any field data if using a static Singleton State
  otherwise use a dynamic state object allocation for them because each state object
  is for each entity object
``` cpp
template <class entity_type>
class State
{
public:
virtual void Enter(entity_type*)=0;
virtual void Execute(entity_type*)=0;
virtual void Exit(entity_type*)=0;
virtual ~State(){}
};
class Miner : public BaseGameEntity
{
private:
State<Miner>* m_pCurrentState;
State<Miner>* m_pPreviousState;
State<Miner>* m_pGlobalState;
...
public:
void ChangeState(State<Miner>* pNewState);
void RevertToPreviousState();
void Update()const
{
//if a global state exists, call its execute method
if (m_pGlobalState) m_pGlobalState->Execute(m_pOwner);
//same for the current state
if (m_pCurrentState) m_pCurrentState->Execute(m_pOwner);
}
//change to a new state
void ChangeState(State<entity_type>* pNewState)
{
assert(pNewState &&
"<StateMachine::ChangeState>: trying to change to a null state");
};//keep a record of the previous state
m_pPreviousState = m_pCurrentState;
//call the exit method of the existing state
m_pCurrentState->Exit(m_pOwner);
//change state to the new state
m_pCurrentState = pNewState;
//call the entry method of the new state
m_pCurrentState->Enter(m_pOwner);
}
//change state back to the previous state
void RevertToPreviousState()
{
ChangeState(m_pPreviousState);
}
```

## Double buffer
* rendering frame buffer
* to store different field for different object in a frame
``` cpp
class Actor
{
public:
 Actor() : currentSlapped_(false) {}
 virtual ~Actor() {}
 virtual void update() = 0;
 void swap()
 {
 // Swap the buffer.
 currentSlapped_ = nextSlapped_;
 // Clear the new "next" buffer.
 nextSlapped_ = false;
 }
 void slap() { nextSlapped_ = true; }
 bool wasSlapped() { return currentSlapped_; }
private:
 bool currentSlapped_;
 bool nextSlapped_;
};
```
## Bytecode
* use stack and operation to encode different function calls
* all instructions are expressed in a bytecode buffer
* p176 has a untagged union version of following codes
* advice from author:
  >My recommendation is that if you can stick with a single data type, do
that. Otherwise, do a tagged union. That’s what almost every language
interpreter in the world does
``` cpp
//An example for setup data for UI script  
class Value{
  union DataType
  {
    int,
    float,
    string,
  }

  Value(int val)
  {
    this->type = "INT";
    this->data.int = val;
  }

  DataType data;
  string type;

  int AsInt()
  {
    switch (type) {
      case "INT":
        return data.int;
    }
  }
}

class Item
{
  unordered_map<string, Value> map;

  Value Get(string name)
  {
    return map[name];
  }
}
```

## subclass sandbox
* Define behavior in a subclass using a set of operations provided by its base class.

## Type object
* use a object to store attribute value for another object so that different can share same attributes.
``` cpp
class Breed
{
public:
 Monster* newMonster()
 {
 return new Monster(*this);
 }
 // Previous Breed code...
};
And the class that uses them:
class Monster
{
 friend class Breed;
public:
 const char* getAttack()
 {
 return breed_.getAttack();
 }
private:
 Monster(Breed& breed)
 : health_(breed.getHealth()),
 breed_(breed)
 {}
 int health_; // Current health.
 Breed& breed_;
};

Monster* monster = someBreed.newMonster();
```

## Component
* The entity is reduced to a simple container of components.
``` cpp
class GameObject
{
public:
 int velocity;
 int x, y;
 GameObject(InputComponent* input,
 PhysicsComponent* physics,
 GraphicsComponent* graphics)
 : input_(input),
 physics_(physics),
 graphics_(graphics)
 {}
 void update(World& world, Graphics& graphics)
 {
 input_->update(*this);
 physics_->update(*this, world);
 graphics_->update(*this, graphics);
 }
private:
 InputComponent* input_;
 PhysicsComponent* physics_;
 GraphicsComponent* graphics_;
};

GameObject* createBjorn()
{
 return new GameObject(
 new PlayerInputComponent(),
 new BjornPhysicsComponent(),
 new BjornGraphicsComponent());
}
```


## Event queue
* handle different api calls between threads
* do not block caller's thread
* You only need a queue when you want to decouple something in time.
* What goes in the queue?
  * Events: past
  * Message: future
* Who can read from the queue?
  * A single-cast queue:
  * A broadcast queue:
  * A work queue
* Who can write to the queue?
  * one
  * multiple
* What is the lifetime of the objects in the queue?
  * Pass or Share ownership
* asynchronous: event queue
* synchronous: observer pattern

## Service Locator
* to provide a way to find class to use in runtime
 not to call the Singleton function
``` cpp
class Audio
{
public:
 virtual ~Audio() {}
 virtual void playSound(int soundID) = 0;
 virtual void stopSound(int soundID) = 0;
 virtual void stopAllSounds() = 0;
};
class ConsoleAudio : public Audio
{
public:
 virtual void playSound(int soundID)
 {
 // Play sound using console audio api...
 }
 virtual void stopSound(int soundID)
 {
 // Stop sound using console audio api...
 }
 virtual void stopAllSounds()
 {
 // Stop all sounds using console audio api...
 }
};
class Locator
{
public:
 static Audio* getAudio() { return service_; }
 static void provide(Audio* service)
 {
 service_ = service;
 }
private:
 static Audio* service_;
};

Audio *audio = Locator::getAudio();
audio->playSound(VERY_LOUD_BANG);

ConsoleAudio *audio = new ConsoleAudio();
Locator::provide(audio);
```

## data locality
* cache misses detection: The cheap way to profile is to manually add a bit of instrumentation that checks how much time has elapsed between two points in the code, hopefully using a precise timer.
* To arrange and allocate data properly in order to avoiding memory cache missing
  * Contiguous arrays
  * Packed data
  * Hot/cold splitting

## Dirty flag
* to avoid time wasting operation like recalculation and memory copy
* use bit to mark them
``` cpp
class GraphNode
{
public:
 GraphNode(Mesh* mesh)
 : mesh_(mesh),
 local_(Transform::origin()),
 dirty_(true)
 {}
 // Other methods...
private:
 Transform world_;
 bool dirty_;
 // Other fields...
};
void GraphNode::render(Transform parentWorld,
 bool dirty)
{
 dirty |= dirty_;
 if (dirty)
 {
 world_ = local_.combine(parentWorld);
 dirty_ = false;
 }
 if (mesh_) renderMesh(mesh_, world_);
 for (int i = 0; i < numChildren_; i++)
 {
 children_[i]->render(world_, dirty);
 }
}
```
## Object pool
* Define a pool class that maintains a collection of reusable objects.
``` cpp
template <class TObject>
class GenericPool
{
private:
 static const int POOL_SIZE = 100;
 TObject pool_[POOL_SIZE];
 bool inUse_[POOL_SIZE];
};
```
------
## [Online version](http://gameprogrammingpatterns.com/contents.html)
## Unity related Patterns
  * Game Loop Pattern: The Unity framework has a complex game loop detailed in a wonderful illustration you can find by searching for “MonoBehaviour Lifecycle”.
  * Update Method Pattern: The Unity framework uses this pattern in several classes, including MonoBehaviour.
  * Component Pattern: The Unity framework’s core GameObject class is designed entirely around components.
  * Service Locator Pattern: The Unity framework uses this pattern in concert with the Component pattern (p. 213) in its GetComponent() method.
