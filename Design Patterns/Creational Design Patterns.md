- focus on object creations
- controls the way objects are created 
- By specifying a common interface 
- always return an object pointer btw

 **USUALLY Compose of**
- *creator* class
- *product* class

Example :

- [[Abstract Factory]]
- [[Factory]]
- [[Builder]]
- [[Prototype]]
- [[Singleton]]


| Pattern              | Desc                                                                       | Special Class/methods/Notes                                        |
| -------------------- | -------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| [[Abstract Factory]] | Replace constructors of a family of objects                                | createX()/createY()/createZ()                                      |
| [[Factory]]          | Replace constructors of a single objects                                   | createX()                                                          |
| [[Builder]]          | Replace constructors for a single object and allow a complex build process | Director ( a special list of predefined objects),, method chaining |
| [[Prototype]]        | Replace copy constructor                                                   | clone()                                                            |
| [[Singleton]]        | Allow one instance of object                                               | getInstance()                                                      |
