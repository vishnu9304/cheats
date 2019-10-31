### What is a go module?

A module is a way of packaging the software as simple as that.

A go module is not atomic and it can contain many packages inside like a repository and it is **versioned**.

Go follows something called Semantic Versioning to version the packages. Also called as semver.

### oh wait, then what is the difference between a package and a repository?

A package cannot be sub divided in to working pieces. If you divide the package in to two different files, still you need those two files to represent the whole pacakge.

A package is an atomic thing.

A repository contains many packages that can move around independently.

### What is versioning? Why do we use it? How do we use it?

Software changes over time. We need some kind of standard way of tracking these changes over period of time.

That is where Semantic Versioning comes in to picture. It is a pre straight forward convention of how to name versions.

### Example:

    v1.0.0 

    v1 - Major Version for every single non backward compatible versions.

    v1.0 - Minor Version. We increase it when we add a new feature and not broken any backward Compatiblity.

    v1.0.0 - Patch updates. fixing things that has broken, like security bug fixes.

### Writing programs outside the GOPATH

Command to check default gopath: **go env GOPATH**

### Example:

    package main

    import (
	    "fmt"
        "log"
        "os"

	    "github.com/sirupsen/logrus"
    )

    func main() {
	    _, err := io.Copy(os.Stdout, os.Stdin)
        if err != {
            logrus.Fatal(err)
        }
    }

Here **github.com/sirupsen/logrus** is a third party package.

When you do **go get** it will complain that it is not able to find the gopath. It cannot store code outside gopath.

    Vishnus-Mac:mycat v01$ go get
    
    go get: no install location for directory /Users/v01/dev/mycat outside GOPATH
	For more details see: 'go help gopath'

Go module comes to rescuse the above scenario. We are going to go from a **directory having go code to directory having go module**

    Vishnus-Mac:mycat v01$ go mod init

    go: cannot determine module path for source directory /Users/v01/dev/mycat (outside GOPATH, no import comments)

Again it will complain as you are outside the gopath and it doesn't know what module you are initalizing.

We need to provide the **import path**

    Vishnus-Mac:mycat v01$ go mod init github.com/vishnu9304/mycat
    go: creating new go.mod: module github.com/vishnu9304/mycat

    Vishnus-Mac:mycat v01$ cat go.mod
    module github.com/vishnu9304/mycat

    go 1.12

When you run **go build** it will pull all the dependencies and mentioned in our go code.

    Vishnus-Mac:mycat v01$ go build
    
    Vishnus-Mac:mycat v01$ ls
    go.mod	go.sum	main.go	mycat

    Vishnus-Mac:mycat v01$ cat go.mod
    module github.com/vishnu9304/mycat

    go 1.12

    require github.com/sirupsen/logrus v1.4.2

    Vishnus-Mac:mycat v01$ cat go.sum
    github.com/davecgh/go-spew v1.1.1/go.mod h1:J7Y8YcW2NihsgmVo/mv3lAwl/skON4iLHjSsI+c5H38=
    github.com/konsorten/go-windows-terminal-sequences v1.0.1/go.mod h1:T0+1ngSBFLxvqU3pZ+m/2kptfBszLMUkC4ZK/EgS/cQ=
    github.com/pmezard/go-difflib v1.0.0/go.mod h1:iKH77koFhYxTK1pcRnkKkqfTogsbg7gZNVY4sRDYZ/4=
    github.com/sirupsen/logrus v1.4.2 h1:SPIRibHv4MatM3XXNO2BJeFLZwZ2LvZgfQ5+UNI2im4=
    github.com/sirupsen/logrus v1.4.2/go.mod h1:tLMulIdttU9McNUspp0xgXVQah82FyeX6MwdIuYE2rE=
    github.com/stretchr/objx v0.1.1/go.mod h1:HFkY916IF+rwdDfMAkV7OtwuqBVzrE8GR6GFx+wExME=
    github.com/stretchr/testify v1.2.2/go.mod h1:a8OnRcib4nhh0OaRAV+Yts87kKdq0PP7pXfy6kDkUVs=
    golang.org/x/sys v0.0.0-20190422165155-953cdadca894 h1:Cz4ceDQGXuKRnVBDTS23GTn/pU5OE2C0WrNTOYK1Uuc=
    golang.org/x/sys v0.0.0-20190422165155-953cdadca894/go.mod h1:h1NjWce9XRLGQEsW7wpKNCjG9DtNlClVuFLEZdDNbEs=

Under **go.mod** you can see a new line **require github.com/sirupsen/logrus v1.4.2**. Our module depends on that module.

You will see one more additional file **go.sum** which contains a lot of info.

On top of the version we are using we also have a HASH **J7Y8YcW2NihsgmVo/mv3lAwl/skON4iLHjSsI+c5H38=**

It's a cryptographic hash to track if something that got changed over time.

### Commonly used commands

Command to fetch a module with the latest tag:

    go get -u github.com/sirupsen/logrus

Let's do some clean up: **go mod tidy**

When you do a **go ge**t or **go build** your test files are ignored. **go mod tidy** will also find the modules that your tests are depending on.

### Where are these downloaded modules stores?

It will be stored under **GOPATH** /go/pkg/mod

### How to identify why we are depending on some module?

    Vishnus-Mac:mycat v01$ go mod why github.com/pmezard/go-difflib
    # github.com/pmezard/go-difflib
    (main module does not need package github.com/pmezard/go-difflib)

    Actually we are not using github.com/pmezard/go-difflib directly. 
    We are using some module inside it. 
    To identify that we need to add -m flag.

    Find something that is in github.com/pmezard/go-difflib

    Vishnus-Mac:mycat v01$ go mod why -m github.com/pmezard/go-difflib
    # github.com/pmezard/go-difflib
    github.com/vishnu9304/mycat
    github.com/sirupsen/logrus
    github.com/sirupsen/logrus.test
    github.com/stretchr/testify/assert
    github.com/pmezard/go-difflib/difflib
    
### Migrating modules to use go mod.

    Step 1: Clone the repo.

    Step 2: go mod init

    Step 3: go mod tidy

    Step 4: go get ./...