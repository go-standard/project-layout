# project-layout
Standard Go Project Layout

# Notes

I created this nearly empty repo as a joke, but I continue to see bad patterns in packages in workplaces (namely mine).
Here are some rules which may help you.

## main

Main package in the root is ok, but probably not what you want longer term.
Make your root package something importable and place your main package in a `cmd/<execname>` directory.

**For many small to medium size projects these are the only 2 packages you need.**

## internal

Unless you are shipping code to external teams, you probably don't need this and it will be more trouble than it is worth.

Do you work on a team which claims to be agile?

_hot take:_ Unexported code is anti-agile.

Go read the agile software manifesto.[^1]

Every single one of those values suggests exporting as much as possible.

## pkg

Never.
Any package you put into a `pkg` directory can be moved the root.

> But Jay, I really want pkg..

NO!

## util

NO!

Those functions, types, and methods which you think belong in a separate util (or common, or shared, or lib) package simply don't.

> But Jay, I'm more conformtable with...

Let me guess, you have more experience writing Java, Python, Ruby, JavaScript, TypeScript, or C# than you do writing C, C++, or Perl.
Please consider that package structuring is a 

## kubernetes is not a good Go code reference project

Never use kubernetes code as an example of Go idioms.
It was started as a Java project and then converted.
It is NOT idiomatic Go. 
The kubernetes Go codebase violates basic Go idioms such as accept interfaces return concrete types.

## signs your project layout has too many packages

If you notice that in many of your files/packages you repeatedly are importing the same packages from the same module, it may be a sign that these packages should be merged. e.g.

In myproj/cmd/cmda:
```go
import (
  ...
  "myproj/http"
  "myproj/json"
)
```

In my proj/cmd/cmdb:
```go
import (
  "myproj/http"
  "myproj/json"
)
```

If they are never used without one another, then they should be together.

> But they could be used without one another!

Yes, and what value would that add?

> 😾

I'll answer my own question: None.


## why does this matter?

Import cycles are not allowed by the Go compiler.
By creating too many packages you can easily put yourself into a situation where you need to shuffle things around just to prevent a cycle.

## See also

* [^1] http://agilemanifesto.org
* https://duckduckgo.com/?q=Go+import+cycles&t=osx&ia=web
* https://go.dev/doc/modules/managing-dependencies
* https://dev.to/jrwren/how-to-write-go-47nk
