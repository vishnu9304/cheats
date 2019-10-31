### How to format the code?    

     go fmt main.go

     go fmt -d main.go      This command will show the difference.
    
### How to build a binary for windows (Cross compilation) architecture from Mac?

    GOOS=windows go build main.go

### What will **go install** do ?

It will compile the code and install the resulting binary under **$GOPATH/bin** directory.

### What will **go get** do?

    go get github.com/golang/example/hello

It will go to github.com download the code compliles it and place the binary under **$GOPATH/bin** 

### Extracting info from go packages. **go doc and go list**

    Vishnus-Mac:test v01$ ls
    main.go
    
    Vishnus-Mac:test v01$ go list -f '{{ .Name }}: {{ .Doc }}: {{ .Imports }}'
    main: This is a demo!: [fmt]

    Vishnus-Mac:test v01$ go list -f '{{ join .Imports "\n" }}' fmt
    errors
    internal/fmtsort
    io
    math
    os
    reflect
    strconv
    sync
    unicode/utf8

    go list -f '{{ .Doc }}' fmt     // To list the "fmt" package documentation.

### go doc

    go doc fmt  // To check documentation for fmt package.

    godoc -http :6060   // To run http doc server.

### **errcheck** tool

Run  **errcheck** command under the directory where the go files are kept. It will display any unhandled errors.


Refer: https://youtu.be/uBjoTxosSys


    