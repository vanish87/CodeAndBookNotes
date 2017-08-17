### Non-continuous collision detection:
– Detection
• Are two shapes overlapping?

– Resolution
• Make them not overlapping anymore

– Response
• Make them bounce off each other in some believable way
### Continuous collision detection:
– Analytic detection
• Compute where two shapes will overlap

– Translation
• Translate them to exactly that point

– Response
• Same as above

### collision class
```cpp
public class Entity {
public:
  Cylinder getCylinder();
  void onCollide(Entity *e, Vector3 mtv);
}
```

### Useful tips:
**“I will not check for or respond to collisions in an Entity’s onTick() method”**
