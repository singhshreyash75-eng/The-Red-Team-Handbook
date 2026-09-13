# Go

> Go programming fundamentals and why Go is widely used for modern networking and security tooling.

## What is Go?

**Go**, also called **Golang**, is a compiled programming language created at Google.

It is widely used for:

```text
Networking
Cloud infrastructure
CLI tools
Concurrent applications
Security tooling
Web services
DevOps
```

Many tools from earlier modules are written in Go.

Examples include tools in ecosystems such as:

```text
Subfinder
httpx
Nuclei
Katana
Gobuster
```

---

# Why Go for Security Tools?

Go provides:

```text
Fast compilation
Single binaries
Cross-platform support
Concurrency
Strong networking libraries
Good performance
Simple deployment
```

A Go tool can often be compiled into one executable without requiring the target system to install a language runtime.

---

# Check Installation

```bash
go version
```

---

# First Program

Create:

```text
main.go
```

Code:

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, security!")
}
```

Run:

```bash
go run main.go
```

Compile:

```bash
go build main.go
```

This creates a native executable.

---

# Program Structure

Basic structure:

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello")
}
```

Important:

```text
package main
      ↓
Executable program

func main()
      ↓
Program entry point
```

---

# Variables

Explicit:

```go
var domain string = "example.com"
```

Short declaration:

```go
domain := "example.com"
```

Print:

```go
fmt.Println(domain)
```

---

# Common Types

```text
string
int
float64
bool
byte
rune
```

Example:

```go
name := "Alice"
port := 443
alive := true
```

---

# Constants

```go
const DefaultPort = 443
```

Constants cannot be reassigned.

---

# Arrays

Fixed size:

```go
var ports [3]int = [3]int{80, 443, 8080}
```

More commonly, Go programs use slices.

---

# Slices

```go
ports := []int{80, 443, 8080}
```

Loop:

```go
for _, port := range ports {
    fmt.Println(port)
}
```

Slices are dynamic and heavily used.

---

# Maps

Maps store key/value pairs.

```go
services := map[int]string{
    80:  "HTTP",
    443: "HTTPS",
}
```

Access:

```go
fmt.Println(services[443])
```

---

# If Statements

```go
port := 443

if port == 443 {
    fmt.Println("HTTPS")
} else {
    fmt.Println("Other")
}
```

Parentheses around the condition are not required.

---

# For Loop

Go primarily uses:

```text
for
```

for looping.

Example:

```go
for i := 0; i < 5; i++ {
    fmt.Println(i)
}
```

---

# Range

Iterate over a slice:

```go
hosts := []string{
    "example.com",
    "api.example.com",
}

for _, host := range hosts {
    fmt.Println(host)
}
```

---

# Functions

```go
func greet(name string) {
    fmt.Println("Hello", name)
}
```

Call:

```go
greet("Alice")
```

---

# Return Values

```go
func add(a int, b int) int {
    return a + b
}
```

Use:

```go
result := add(2, 3)
```

---

# Multiple Return Values

Go supports multiple returns.

Example:

```go
func divide(a, b int) (int, error) {
    if b == 0 {
        return 0, fmt.Errorf("division by zero")
    }

    return a / b, nil
}
```

This is commonly used for error handling.

---

# Error Handling

Go does not generally rely on exception handling in the same style as Python.

Typical:

```go
result, err := someFunction()

if err != nil {
    fmt.Println("Error:", err)
    return
}
```

Conceptually:

```text
Function
 ↓
Value + Error
 ↓
Check Error
```

---

# Structs

Structs group related data.

```go
type Target struct {
    Host string
    Port int
}
```

Create:

```go
target := Target{
    Host: "example.com",
    Port: 443,
}
```

---

# Methods

Methods can be associated with types.

```go
func (t Target) Print() {
    fmt.Println(t.Host, t.Port)
}
```

---

# Pointers

Go supports pointers.

```go
x := 10
p := &x
```

`p` stores the address of `x`.

Dereference:

```go
fmt.Println(*p)
```

Go's pointer model is more constrained than languages such as C.

---

# Packages

Code is organized into packages.

Example:

```go
import (
    "fmt"
    "net/http"
)
```

Go's standard library is one of its major strengths.

---

# HTTP Client

Go provides strong built-in HTTP support.

Example:

```go
package main

import (
    "fmt"
    "net/http"
)

func main() {
    resp, err := http.Get("https://example.com")
    if err != nil {
        fmt.Println(err)
        return
    }
    defer resp.Body.Close()

    fmt.Println(resp.Status)
}
```

Use only authorized endpoints for automated security testing.

---

# defer

`defer` schedules an operation for execution when the surrounding function returns.

Example:

```go
defer resp.Body.Close()
```

This is commonly used for cleanup.

---

# Networking

Go's standard library includes packages such as:

```text
net
net/http
net/url
crypto/tls
```

This makes it particularly suitable for networking/security tools.

---

# Goroutines

One of Go's most important features is:

```text
Goroutines
```

A goroutine is a lightweight concurrent function execution.

Example:

```go
go checkHost("example.com")
```

Conceptually:

```text
Main Program
    |
    +--> Goroutine 1
    +--> Goroutine 2
    +--> Goroutine 3
```

This is one reason Go is popular for fast reconnaissance tools.

---

# Concurrency

Suppose you need to check many authorized hosts.

Sequential:

```text
Host 1
 ↓
Host 2
 ↓
Host 3
```

Concurrent:

```text
      +--> Host 1
Program
      +--> Host 2
      +--> Host 3
```

Concurrency can significantly improve network-tool performance.

---

# Channels

Channels allow goroutines to communicate safely.

Example:

```go
ch := make(chan string)
```

Send:

```go
ch <- "done"
```

Receive:

```go
msg := <-ch
```

Conceptually:

```text
Goroutine A
     |
   Channel
     |
Goroutine B
```

---

# WaitGroup

When running multiple goroutines, programs often need to wait for them to finish.

Go's synchronization packages provide tools such as:

```text
sync.WaitGroup
```

Conceptually:

```text
Start Workers
     ↓
Wait
     ↓
All Finished
     ↓
Exit
```

---

# Concurrency Safety

Fast does not mean unlimited.

A security tool should control:

```text
Concurrency
Rate
Timeouts
Retries
```

Otherwise it can overwhelm:

```text
Target
Network
Local machine
```

---

# File Reading

Example:

```go
data, err := os.ReadFile("targets.txt")
if err != nil {
    fmt.Println(err)
    return
}

fmt.Println(string(data))
```

---

# Line-by-Line Input

For large target lists, scanners commonly process input incrementally rather than loading everything into memory.

Useful standard packages include:

```text
bufio
os
```

---

# Command-Line Arguments

```go
os.Args
```

contains command-line arguments.

For professional CLI programs, Go also provides:

```text
flag
```

and third-party CLI libraries are common.

---

# Environment Variables

Read:

```go
value := os.Getenv("API_KEY")
```

Avoid printing secret environment variables into logs.

---

# JSON

Go has built-in JSON support:

```text
encoding/json
```

This is extremely useful for:

```text
API clients
Scanner output
Configuration
Automation
```

---

# Regular Expressions

Package:

```text
regexp
```

Useful for controlled pattern matching and data extraction.

---

# Go Modules

Modern Go dependency management uses:

```text
Go Modules
```

Initialize:

```bash
go mod init example
```

Dependencies are tracked through files such as:

```text
go.mod
go.sum
```

---

# Installing Go Tools

Many Go-based tools can be installed using:

```bash
go install <module>@<version>
```

Prefer explicit/reviewed versions in reproducible environments rather than blindly installing arbitrary latest code.

---

# Cross Compilation

Go can compile binaries for different operating systems and architectures.

Conceptually:

```text
Source Code
    ↓
Go Compiler
    ↓
Linux Binary
Windows Binary
macOS Binary
```

This is useful for portable administration and security utilities.

---

# Static Binaries

Go applications can often be deployed as relatively self-contained binaries.

Benefits:

```text
Easy distribution
Fewer runtime dependencies
Simple container deployment
```

---

# Go in Security Tooling

A common architecture:

```text
Input Targets
     ↓
Concurrent Workers
     ↓
Network Requests
     ↓
Parse Responses
     ↓
Structured Output
```

This maps naturally to Go.

---

# Example Safe HTTP Checker

```go
package main

import (
    "fmt"
    "net/http"
    "time"
)

func main() {
    client := http.Client{
        Timeout: 5 * time.Second,
    }

    resp, err := client.Get("https://example.com")
    if err != nil {
        fmt.Println("Error:", err)
        return
    }
    defer resp.Body.Close()

    fmt.Println("Status:", resp.StatusCode)
}
```

Important feature:

```text
Timeout
```

Network tools should not wait indefinitely.

---

# Security Considerations

When writing network/security tools:

```text
Validate input
Set timeouts
Limit concurrency
Limit request rate
Handle TLS intentionally
Protect API keys
Avoid command injection
Log safely
Respect scope
```

---

# Go vs Python

```text
Go
 |
Compiled
Fast
Concurrency-oriented
Single-binary deployment
```

versus:

```text
Python
 |
Interpreted
Very fast development
Large security ecosystem
Excellent scripting
```

Both are valuable.

---

# Go vs Bash

Use Bash when:

```text
Primarily chaining commands
```

Use Go when:

```text
Building larger
high-performance
concurrent tooling
```

---

# Key Takeaway

Go's security-tooling strengths are:

```text
COMPILED
   +
FAST
   +
PORTABLE
   +
NETWORKING
   +
CONCURRENCY
```

The key concepts to learn are:

```text
Variables
Slices
Maps
Functions
Structs
Error handling
Packages
HTTP/networking
Goroutines
Channels
```

Go becomes particularly useful when a Bash/Python prototype grows into a tool that needs:

```text
Performance
Concurrency
Reliability
Easy distribution
```
