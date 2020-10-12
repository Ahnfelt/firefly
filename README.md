# A language in development

Firefly compiles itself: https://github.com/Ahnfelt/firefly-boot

# A taste of Firefly

```
function main(system: System): Task[Unit] {
  copyFile(system.files, "in.txt", "out.txt")
}

function copyFile(fs: FileSystem, in: String, out: String): Task[Unit] {
  let bytes = fs.readBytes(in)?
  fs.writeBytes(out, bytes)?
}
```

In Firefly, the `main` function gets an instance of `System`, which allows you to do anything. You can then delegate responsibility to other functions by passing them either `system` or one of its fields, such as `system.files`, which only lets you access the file system. Other examples are `system.network` and `system.environment` etc. 

Firefly has no global state through which such capabilities can leak.

Because there is no global state, all functions are pure unless they are passed or close over impure objects. That means they will reliably return the same value every time they're passed the same arguments.


# Traits

```
trait T: ReadJson {
  read(json: Json): Result[T]
}

trait T: WriteJson {
  write(value: T): Json
}

instance String: ReadJson {
  read(json) {
    | JString(s) => Ok(s)
    | j => NotOk("Expected JSON string, got " ++ show(j))
  }
}

instance String: WriteJson {
  write(value) {
    JString(value)
  }
}

function main(system) {
  let configuration = Json.readFile[Configuration](system.files, "configuration.json")
  system.out.writeLine(configuration("greeting").string)
}
```


## Lambdas and pattern matching

```
list.map {x => x * x}

list.foldLeft(0) {x, y => x + y}

list.foldLeft(0) {_ + _}

function unify(type1: Type, type2: Type): Unit {
  | TVariable(i1), TVariable(i2); i1 == i2 =>
  | TVariable(i), _ =>
    assert(!occursIn(i, t2))
    bind(i, t2)
  | _, TVariable(i) =>
    assert(!occursIn(i, t1))
    bind(i, t1)
  | TConstructor(name1, generics1), TConstructor(name2, generics2) =>
    assert(name1 == name2 && generics1.size == generics2.size)
    generics1.zip(generics2).each {| (t1, t2) => unify(t1, t2) }
}
```

  * Lambda functions may directly pattern match on their arguments.
  * The pipe `|` is used to indicate a case. 
  * The `;` indicates a pattern guard. 
  * When there's a single case consisting of only variable names, the pipe can be left out. 
  * Function bodies can pattern match on the arguments of the function as well. 
  * Lambdas are always enclosed in `{}` and support multiple statements. 
  * In expressions, `_` is an anonymous parameter, always belonging to the nearest enclosing `{}`. 
  * In patterns, `_` ignores a value.  
  * Lambdas can occur after a field or a call, in which case they become an extra argument to the method call.
