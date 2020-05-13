# Firefly
*Kiss subtyping goodbye!*

A simple and safe alternative to Java.

*Firefly combines a small set of simple features -- all of which were invented several decades ago.*

**This is a draft -- claims made below are merely tentative goals.**


## Firefly is safe

### Problem: Ambient Authority

In most programming languages, every piece of code can silently read from your file system, access the internet, and generally misbehave. It's not feasible to vet all the dependencies that a modern application might have. Is the `BCrypt` implementation you depend on secretly sending your passwords somewhere? Have you checked?

### Solution: Object Capabilities

```
function main(system: System): Unit {
  copyFile(system.files, "in.txt", "out.txt")
}

function copyFile(fs: FileSystem, in: String, out: String) {
  let bytes = fs.readBytes(in)
  fs.writeBytes(out, bytes)
}
```

The `main` function gets an instance of `System`, which allows you to do anything, and you can then delegate responsibility to other functions by passing them either `system` or one of its fields, such as `system.files`, which only lets you access the file system. Other examples are `system.network` and `system.environment` etc.

Firefly has no global state, so it's not possible to accidentally make such capabilities globally available. 

