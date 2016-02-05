## Golang OPPs Concept

Golang is officially created by __Google__ and appeared for the first time in 2009. The language designers have years of experience and knowledge and they are genius people such as Ken Thompson, Rob Pike, Robert Griesemer etc. It helped Golang to be __Fast__, __Productive__ and most importantly __Fun__.

There are multiple blogs, presentations and videos that talks about the pros and cons of Golang OOPs concepts. Few talking points are always there, mainly when Golang is compared to other OOPs languages. The concepts such as inheritance via composition, go interface, having method for a value or a type, this could seem really weird for a Object Oriented Programming point of view. But It is really important for everyone to think in terms of Go and not the languages they are leaving behind.

So the question is always there that how well golang does score as an object oriented language, and why it’s not actually important for golang.

#### Procedural | Object Oriented

Currently the programming language world is separated in two different forces, Procedural and Object-oriented. If I compare Golang with the Star-Wars, it could be imagined as a peace maker between the Jedi order and Sith. Golang is neither completely procedural, nor object-oriented. It could be considered as a light object oriented programming language. It does have encapsulation and type member functions, support polymorphic and code reusability. Where it also encourage to follow the good programming practice by supporting method oriented approach to certain scenario, mainly when you are doing network programming. As per go performance concern it is almost as fast as ``C``, where other Object Oriented languages are far behind. What makes Golang an obvious choice over the others.

#### Golang Structure | Class
Golang structures are very simple and it is quite streamlined with ``C``. Here is a very common example of three Golang structure.
```go
type Animal struct {
    ScientificName string
    Genre string
}

type Cat struct {
    basic Animal
    MeowStrength int
}

type Dog struct {
    basic Animal
    BarkStrength int
}
```
Here animal is a base structure that holds the properties of an animal. Now Cat and Dog both should have the properties of Animal, which can be done via composition of the specific type. This is exactly how we reuse code in procedural languages like ``C``.

Typically in OOPs the concept of Class is defined as a composition of properties and method. The method defines the behaviour of the specific type. Like animal does breath and they also make noise. Now cat and dog both make noise differently, which defines the make noise behaviour of both of the different Animal. This is done via overriding the Animal property. 

Above example do inherit the properties but it doesn't inherit the behaviours. Here is an example of how we define behaviours and properties of a type and inherit it in a derived type.
```go
type Animal struct {
        Name  string
        Genre string
}

type Cat struct {
        Animal
        MeowStrength int
}

type Dog struct {
        Animal
        BarkStrength int
}

type GoldFish struct {
        Animal
        SwimSpeed int
}

func (animal Animal) GetName() {
        fmt.Printf("%s ", animal.Name)
}

func (animal Animal) MakeSound() {
        animal.GetName()
        fmt.Printf("I dont know how to make sound :( !\n")
}

func (cat Cat) MakeSound() {
        cat.GetName()
        fmt.Printf("Meowwwww at %d !\n", cat.MeaoStrength)
}

func (dog Dog) MakeSound() {
        dog.GetName()
        fmt.Printf("Voouuuu at %d !\n", dog.BarkStrength)
}

func (fish GoldFish) Swim() {
        fish.GetName()
        fmt.Printf("Swimming at %d !\n", fish.SwmSpeed)
}

func main() {
        var dog Dog = Dog{Animal{"dog", "mammal"}, 2}
        var cat Cat = Cat{Animal{"cat", "mammal"}, 3}
        var fish GoldFish = GoldFish{Animal{"goldfish", "fish"}, 4}
        cat.MakeSound()
        dog.MakeSound()
        fish.MakeSound()
        fish.Swim()
}

/* Output:
cat Meowwwww at 3 !
dog Voouuuu at 2 !
goldfish I dont know how to make sound :( !
goldfish Swimming at 4 !
*/
```
[Example 1 PlayGround ^](https://play.golang.org/p/bUvcMSUCTC)

Golang uses the casing of the first letter of the name to define the private and public scope of variable, method and structure and any other type. This is very simple yet complete way. If a property in a structure starts with a small cased, it could only be accessed directly within that package. 
```go
// This struture visibility is only within package
type animal struct{
	name string // It could only accessed directly within this package
    	gener string		
}

// This structure is public
type Dog struct {
        animal
        BarkStrength int
}

// this method has only package scope. It can not be called from a different package.
func (animal animal) getName() {
        fmt.Printf("%s ", animal.name)
}

// This method is public and can be called from a different package
func (dog Dog) MakeSound() {
        dog.getName()
        fmt.Printf("Voouuuu at %d !\n", dog.BarkStrength)
}
```
Package private global variable, and package private type, could be very handful while implementing singleton design pattern in golang. Below is an simple example.
```go
package logger

var glogger *logger = nil 

// This structure visibility is only within package. It could only be initialized within the package
type logger struct{
	path string
	// .. other logger properties
}

// this package private function initiate a logger for the first time
func (logger_obj *logger) initiate() {
	// Does the initiation
} 

// This function get/initiate glogger and set the Object
func InitLogger(path string){
	if (glogger == nil) {
		glogger = &logger{}
		glogger.path = path
		// set other properties
		glogger.initiate()
	}
}

// This is a public function use global glogger to log
func Log(data string) {
	// use glogger to log the string
}
```
But this way we make the Class completely hidden from other packages. For a proper singleton class example, you could visit [Marcio Castilho](http://marcio.io/2015/07/singleton-pattern-in-go/).

Golang methods are __non-virtual__ in nature, which means it doesn't actually override the method. All inherited methods are called on the struct type and the main type cannot see or know about derived methods. Here everything is non-virtual. When working with struct and embedding, everything is statically linked and all the references are resolved at compile time. So in Golang you can access the embedded object's behaviour. 

So inheritance via composition in Golang does not generalize the object in particular but it adds some features embedding another type. But the two types remain distinct. Below is an example of subtyping in Golang using earlier example.
```go
cat.Animal.MakeSound() 	// output will be  "I dont know how to make sound :( !"
cat.MakeSound()			// output will be  "Meawwwww at 3 !"
```
[Example 2 PlayGround ^](https://play.golang.org/p/hBeMdsPlfA)

Golang supports multiple inheritance. When it comes to composition, it is quite obvious that any language should support multiple embedding so does Golang. Golang supports embedding of different object into one. 
```go
type Animal struct {
	Name  string
    	Genre string	
}

func (animal Animal) GetName() {
    fmt.Printf("%s ", animal.Name)
}

func (animal Animal) MakeSound() {
    animal.GetName()
    fmt.Printf("I dont know how to make sound :( !\n")
}

type ScientificDetails struct {
	Name string
}

func (animal ScientificDetails) GetName() {
    fmt.Printf("%s ", animal.Name)
}

type Cat struct {
	Animal
	ScientificDetails
	MeaoStrength int
}

func main() {
    var cat Cat = Cat{Animal{"cat", "mammle"}, ScientificDetails{"Felis Catus"}, 3}
    cat.Animal.GetName()			
    cat.ScientificDetails.GetName() 
}
/* Output: 
cat Felis Catus 
*/
```
[Example 4 PlayGround ^](https://play.golang.org/p/fekvUnYTz_)
Golang allows you to create a "Diamond" inheritance diagram, and only will complain when you try to access a parent's class field ambiguously. As Golang inheritance is resolved at compile time, Golang compiler could alert the ambiguity at compile time.
```go
cat.GetName() // This will produce a compile time error because of the ambiguity (ambiguous selector cat.GetName)
```
[Example 4 PlayGround ^](https://play.golang.org/p/d3NxQHXIEy)

#### Golng Interface

Overriding is a comparatively slow process that is done in runtime, and inheriting a behaviour and overriding a behaviour is a completely different problem. Golang doesn't support the true subtyping, which makes it faster. If this was truly subtyping then the anonymous field would cause the outer type to become the inner type. In Golang this is simply not the case. So the below thing doesn't exactly workout.
```go
var cat Cat = Cat{Animal{"cat", "mammal"}, 3}
var catref Animal = cat // Compile time error: cannot use cat (type Cat) as type Animal in assignment
catref.MakeSound()
```
[Example 3 PlayGround ^](https://play.golang.org/p/PDucPJjRK-)

Now what exactly that makes OOPs distinct from procedural language object's behaviour is the __runtime polymorphism__, or dynamic method dispatch.  This allows extending usage of method by allowing it for various types. What means that ``Cat`` and ``Dog`` both can be referred as an ``Animal``. Now whenever I'm referring a ``Cat`` using an ``Animal`` type and invoking a ``Animal``'s behaviour, it should dispatch the ``Cat``'s behaviour rather than the ``Animal``'s one. To achive this Golang __interface__ is used. 

Golang interfaces are available as a type in Golang. It is a class, with no fields or properties, and all virtual method. Interface can be used as variable type and a parameter type, or a return type. A Golang interface could be embedded (inherited) into another interface. Any structure that defines a behaviour defined by an interface, implements the interface.

To be Continued ...
