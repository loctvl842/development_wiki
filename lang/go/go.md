# Go / GoLang

## Modules

**1. Initialize a module**

Start a new project by creating a module

```bash
go mod init <module-name>
```

This command creates a `go.mod` file in the root of the project.

**2. Add a dependency**

Once the module is initialized, you can start adding dependencies to your project. You can do this by importing packages in your Go code. For example:

```go
import "github.com/gin-gonic/gin"
```

After adding the import, run the following command to download the dependencies:

```bash
go get <module-name>
```
