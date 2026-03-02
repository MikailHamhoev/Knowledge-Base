* Toolchain

** Installation, Go Modules, and GOPATH

*** Installation
Package for speicfic OS.

*** GOROOT
An environment variable that points to the directory where Go is installed (compiler and standard libraries). Usually is set automatically.

*** GOPATH (Legacy)
All Go code and dependencies had to be placed inside GOPATH/src.

*** Go Modules (Current Standard)
Built-in dependency management system. Allows working outside GOPATH. A project is initialized with go mod init MODULE_NAME, which creates a go.mod file. The go.mod file defines module path and lists all the dependencies required for the project.

** Compiler and Utilites

*** go build
Reads the go.mod and compiles the Go source code with required dependencies to the statically linked binary (executable) specific to current OS (by default).

*** go fmt
Formats code according to Go style guidelines.

*** go vet
Static analysis tool.

*** go mod
Parent command for managing dependencies.

**** go mod tidy
Adds new and removes unused depencies.

**** go mod vendor
Creates a vendor directory containing copies of all dependencies

** Static Binary Compilation
All used libraries are compiled alongside with the code, so the executable can be shared and run easily.

** Runtime
Manages Scheduler (goroutines managing) and Garbage Collector (GC)