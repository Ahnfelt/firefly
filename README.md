# Firefly
*Kiss subtyping goodbye!*

**This is a draft -- claims made below are merely tentative goals.**

Firefly is a safe, imperative/functional programming language with type inference. It seeks to improve the state of high level imperative programming. It builds on a set of features that's radically different from Java, yet allows a familiar style of programming.

*Firefly combines a small set of simple features -- all of which were invented several decades ago.*


## Firefly is safe

Firefly puts you in control over what can happen when you run your program.

In most programming languages, every piece of code can silently read from your file system, access the internet, and generally misbehave. It's not feasible to vet all the dependencies that a modern application might have. Is the `BCrypt` implementation you depend on secretly sending your passwords somewhere? Have you checked?

In Firefly, this is solved by a simple mechanism: The main function gets an instance of System, which allows you to do anything, and you can then delegate responsibility to subfunctions by passing them either system or one of its fields, such as system.files, which only lets you access the file system. Other examples are system.network and system.environment etc.

```
// The main function gets a "system" value that allows it to do anything
function main(system: System): Unit {
    // Here it gives some of this power to copyFile by passing it a FileSystem
    copyFile(system.files, "in.txt", "out.txt")
}

// This function is only able to do things on the file system, but not e.g. access the network
function copyFile(fs: FileSystem, in: String, out: String) {
  let bytes = fs.readBytes(in)
  fs.writeBytes(out, bytes)
}
```

Firefly has no global state, so it's not possible to accidentally make such capabilities globally available. 

