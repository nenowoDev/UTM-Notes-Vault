### Dependency Injection
- form of inversion of control
- you inject the dependency
- design pattern where transfer responsibility of creating object/instance dependency to the container/framework
- loose coupling between components


### Inversion of Control
- decouple of execution and implementation
- every system focus on what it is designed for/focus on goal
- don't make assumptions about what other system do/should do
- replace a system will have no effect on other system ( loose coupling )
- control of object transfer to container


### Type Of Di
- XML
	- can define bean in xml
	- html tag/attributes 
		- bean (id/class/scope)
		- constructor-arg (type/value/ref)
		- property (type/value/ref)
- Java
- Annotation


### Type of injection (self explanatory)
- Constructor Injection 
- Setter Injection  
- Field Injection  

#### Advantage/Dis
- Mandatory/Optional
- Explicit in constructor/setter/field attributes
- immutable/mutable
- flexible/not
- easy to test
- boilerplate (1 constructor/ many setter/ nothing)


### Annotation
- enable it in web.xml
- @Component
	- make the class, a spring-managed bean
		- default to singleton
		- aka init,construct,destroy is all managed by spring
- @Autowired
	- by default, (required=true), can turn off
	- byType injection
		- inject the dependency , spring will search the springbean