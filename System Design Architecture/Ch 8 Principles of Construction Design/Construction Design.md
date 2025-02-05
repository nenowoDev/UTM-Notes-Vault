
- Algorithm Viewpoint
	- Graphical
		- Flow-based 
			- Activity Diagram
		- State-based 
			- UML state Diagram
	- Table-based



- Stylistic Viewpoint
	- Code Formatting 
	- Naming convention
	- Documentation
	- Quality
		- Completeness
		- Correctness
		- Testability
		- Maintainability




## Algorithm viewpoint
- design -> construction phase should be minimal effort
- construction design is lowest lvl of detail
- minimize complexity when coding/construction
- address behavioural perspective
- eval completeness, complexity, testability and maintainability, performance
- graphic/table , non coders can help
- 


Table based

| Condition  | Condition entry  |
| ---------- | ---------------- |
| **Action** | **Action entry** |
- LEDT (limited entry decision table)
- EEDT (extended entry decision table)
- MEDT (mixed entry decision table)

LEDT
- 2^n combination , n is condition
- all true/false only

| Condition                | P1  | P2  | P3  |
| ------------------------ | --- | --- | --- |
| user is teacher          | T   | F   | F   |
| user is student          | F   | T   | F   |
| user is academic officer | F   | F   | T   |
|                          | ... |     |     |
| Discount 10%             | x   |     |     |
| Discount 20%             |     | x   |     |
| Discount 30%             |     |     | x   |

EEDT
- value is not binary(T/F)
- action also can be not binary

| Condition | P1      | P2      | P3               |
| --------- | ------- | ------- | ---------------- |
| User      | Teacher | Student | Academic Officer |
| ..        |         |         |                  |
| Discount  | 10%     | 20%     | 30%              |

MEDT
- combine both

## Sylistic viewpoint
- easier to read, easier to maintain


PDL (Programming design language)
- comment first approach
- easier to create ,review , translate to code
- doesnt care about programming language
- serve as design technique and documentation
example
```pdl
calculate money, receive money param
check if positive value is false
	then return error
if positive value is true,
	then calculate money using formula, 
	add tax
	return new money
```

Code formatting / formatting convention
- whitespace for parantheses/curly braces
```
if ( ){

 }
```
- whitespace for operator,after comma, semicolons
```
x = y + z;
int x, y, z;	
```
- whitespace nested statement/closing bracket
```
if ( ){
	if ( ){
	 }
}
```
- inline bracket/new inline bracket etc..



Naming Convention
- easier to make guess
- ie camelCase,,, BigForClass,, etc...

Documentation Convention
- can be applied universally in different domain/language like naming convention
- basically just comments
- comment like use case description , ie, file,desc,classification,author,post,pre condition,versions

Quality
- Completeness & correctness
	- eval by peer review 
	- unit testing (from use case)
	- audit
- Testability & Maintainability
	- amount of effort required to test/maintain
	- use cyclomatic complexity
		- quantitative justification
		- Thomas J. McCabe , v(G)=e-n+2p  , n vertices , e edges , p connected components
		- Harlan Mills, C= pi + 1 ,, pi number of condition
			- if/while/for loops , pi=1
			- compound (if x and y) , pi=2
			- case statement n branch, pi = n-1
		- Leonhard Euler, 2 = n - e + r ,, r is enclosed region 
			- where Complexity is r+1
		- v(G)/C = 10 is fine



Maintainability
- automated style checker , like eslint, CheckStyle