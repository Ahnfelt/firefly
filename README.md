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
    |JString(s)| Ok(s)
    |j| NotOk("Expected JSON string, got " ++ show(j))
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
