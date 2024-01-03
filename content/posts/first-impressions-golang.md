---
title: "Learning Go: first impressions"
date: 2024-01-02T00:45:28+02:00
draft: false
limmat_temperature: 7.0
images: ["images/golang_logo.jpg"]
tags: ["Tech"]
---

I'm working on a new project to collect hotel booking data regularly, scraping the Booking.com API. The idea is to deploy the scraper using [Google Cloud Functions](https://cloud.google.com/functions) and setup daily scraping using the [Google Cloud Scheduler](https://cloud.google.com/scheduler). I'm writing data to [BigQuery](https://cloud.google.com/bigquery) and plan on using this data later to train a machine learning model to predict hotel room prices.

I started with Python, but got frustrated with the development tools and decided to try something new instead. I've now been learning [Go](https://go.dev/), also known as Golang and I've been quite happy with it so far.

{{< figure src="/images/golang_logo.jpg" width="50%">}}

## Syntax
I find coding syntax in Go very intuitive and easy to read. While it is [strongly-typed](https://stackoverflow.com/questions/2690544/what-is-the-difference-between-a-strongly-typed-language-and-a-statically-typed), you don't need to write type names when declaring variables, as the compiler automatically deduces it. It is also statically typed, meaning the type system is checked at compile time, unlike Python, which is dynamically typed and types are only verified at run time. I actually really like the safety brought by a statically typed language, as this reduces the chances for mistakes and reduces debugging time.

Here is a code snippet in Golang:
```go
// Code is organized around modules. Each module can contain multiple packages.
package main

import (
	"fmt"
	"net/url"
)

const (
	HostURL string = "api.booking.com"
	PathURL string = "/?rooms_list"
)

// There is no formal class definition in Go, instead there's only struct. 
// Structs are a typed collection of fields.
type Scraper struct {
    scrapeURL string
}

// Methods are declared on struct types. This is the closest to a 'class' method.
func (scraper Scraper) checkRequiredParams(q url.Values, requiredParams []string) error {
	for _, param := range requiredParams {
		if q.Get(param) == "" {
			return fmt.Errorf("%s is required", param)
		}
	}
	return nil
}

func main() {
    // Variables can be declared with the := syntax
    scraper := Scraper{}
    urlParams := url.Values{
		"checkin_date":  {"2024-05-12"},
		"checkout_date": {"2024-05-13"},
		"hotel_id":      {"13018"},
    }
    if err := scraper.checkRequiredParams(
        urlParams, 
        []string{"checkin_date", "checkout_date", "hotel_id"}); err != nil {
            // handle error
    }
}
```
One thing I dislike is the `struct` method syntax. I would really have preferred grouping the `struct` methods inside the struct definition itself. I find this increases readability.

### Interfaces
One thing I'm still getting used to are interfaces in Golang. Go just has a different philosophy on how interfaces should be used.

A classic technique for testing is [dependency injection](https://en.wikipedia.org/wiki/Dependency_injection). A typical use case is when using a 'heavy' dependency such as a database. Often it is difficult to use the real database in tests, specially if it's in the cloud. In this case you design your code so that it accepts both the real database client as well as a mock/fake/stub. In Java, C# and C++ this is done with [abstract classes](https://www.ibm.com/docs/en/zos/2.4.0?topic=only-abstract-classes-c). In Go this is done with [interfaces](https://gobyexample.com/interfaces).

For my project, I write to BigQuery and wanted to write tests for it. I was frustrated that the [official BigQuery documentation with Go](https://pkg.go.dev/cloud.google.com/go/bigquery#section-readme) comes with no example on how test this code and I found surprisingly little material on how to do this. The little material I found recommends using [bigquery-emulator](https://github.com/goccy/bigquery-emulator) [[1](https://stackoverflow.com/questions/74878734/mocking-bigquery-for-integration-test)], [[2](https://pkg.go.dev/github.com/yuseferi/bigquery-mock)]. bigquery-emulator is an in-memory implementation of BigQuery, which uses SQLite under the hood. Unfortunately this emulator has two downsides: 
* Does not support the newest API for writing data, which is the [managedwriter API](https://pkg.go.dev/cloud.google.com/go/bigquery/storage/managedwriter).
* Is slow to start on tests.

So I decided to mock the code myself. I was again frustrated to see that the [BigQuery code was not using interfaces at all](https://github.com/GoogleCloudPlatform/golang-samples/blob/main/bigquery/snippets/managedwriter/bigquerystorage_append_rows_pending.go), which makes mocking the code very cumbersome. Little did I know that this is working as intended according to the Golang philosophy of "[Accept Interfaces, Return Concrete Types](https://duncanleung.com/go-idiom-accept-interfaces-return-types/)". Go advocates that it's the users of libraries that should implement the interfaces and only the part that they need, a principle they call "minimal viable interfaces". The idea here is that this reduces the amount of code that they will need to write. Since library authors do not know what parts will be used, creating an interface would mean supporting all use cases. Users would then have to implement of all methods of the interface to be able to use it. I'm not 100% convinced of this argumentation, but I will follow it for now and see it how it goes.

The syntax for interfaces is a bit unintuitive for me (code snippet courtesy of [gobyexample.com]((https://gobyexample.com/interfaces))):
```go
type geometry interface {
    area() float64
}

type rect struct {
    width, height float64
}
type circle struct {
    radius float64
}

func (r rect) area() float64 {
    return r.width * r.height
}
func (c circle) area() float64 {
    return math.Pi * c.radius * c.radius
}

func measure(g geometry) {
    fmt.Println(g.area())
}
```

I would have appreciated some indication that `rect` and `circle` implement `geometry`. In C++ for example this would have been written as

```cpp
class Geometry {
    virtual float area() = 0;
};

class Rectangle : Geometry {
    float area() override {
        // ...
    }
};
```

This is in my opinion helpful for both the user, as they know what it implements, as well as for the programmer, as this ensures they did indeed implement all required methods.

## Development environment
Go has excellent [integration](https://code.visualstudio.com/docs/languages/go) with [Visual Studio Code](https://code.visualstudio.com/), which is what I use to program. Everything just works:
*  IntelliSense: autocompletion of code snippets.
*  Automatic, sane code formatter. Code gets formatted automatically upon save. This was really a pain point for me in Python, as even changing how many spaces are used in indentation is non-trivial [[1](https://stackoverflow.com/questions/59821618/how-to-use-yapf-or-black-in-vscode)], [[2](https://vi.stackexchange.com/questions/23181/how-can-i-configure-black-the-python-code-formatter-to-indent-2-spaces-instead-o)].
*  Blazing fast compilation. Code gets compiled as its being written, highlighting errors automatically, which is very helpful for me as I'm a beginner in Go.
*  Writing code that uses libraries automatically adds them to import declaration, it's magical. If I write `fmt.Println`, the editor automatically adds `import "fmt"`.

## Dependency management
Go has great dependency management and this is a massive advantage of Go compared to other languages. In Python, dependency management is typically done using [pip](https://pypi.org/project/pip/) and [virtualenv](https://virtualenv.pypa.io/en/latest/). Virtualenv creates isolated Python environments, as otherwise when you install a package using `pip` you are installing it for all projects. So the typical flow involves having a Virtualenv for every project you have, then configuring your code editor to also use that specific environment. Deployment used to involve sharing a `requirements.txt`, which contains all packages installed, typically created with `pip freeze`. Nowadays there are more modern tools such as [poetry](https://python-poetry.org/) that ease the burden of dependency management. Nonetheless, this is yet another external tool that you need to use in Python.

Go has built-in [dependency management](https://go.dev/doc/modules/managing-dependencies). Once you initialize a project with `go mod init <project_name>`, it will automatically detect imported libraries. To install them you just have to run `go mod tidy`. Library imports look like this in Golang code:

```go
import "github.com/GoogleCloudPlatform/functions-framework-go/functions"
```

And that's it, it's that simple and I love it.

## Error handling: Python vs Golang
Handling errors gracefully is an important part of writing reliable and maintainable code. Most languages, including Python, Java and C++ handle errors via exception flow with `try` and `catch` statements. Golang however does not have any special syntax for errors and instead they are passed as normal values.

### Python
Can such code fail and if yes, what exception should you catch?
```python
response = requests.get("http://example.com/")
```
I took this code example directly from the [requests library](https://requests.readthedocs.io/en/latest/user/quickstart/), which is one of the most popular libraries for Python ([over 50k stars on GitHub](https://github.com/psf/requests)).

If you try reading the documentation for the function `requests.get`, this is what you find:
```python
def get(url, params=None, **kwargs):
    r"""Sends a GET request.

    :param url: URL for the new :class:`Request` object.
    :param params: (optional) Dictionary, list of tuples or bytes to send
        in the query string for the :class:`Request`.
    :param \*\*kwargs: Optional arguments that ``request`` takes.
    :return: :class:`Response <Response>` object
    :rtype: requests.Response
    """

    return request("get", url, params=params, **kwargs)
```
There's still no indication that this can fail at all. Surprisingly, even if you follow the official [Quickstart guide from the requests library](https://requests.readthedocs.io/en/latest/user/quickstart/), the examples do not include handling exceptions. Since this is a best practice, I would have expected the examples to follow this. Only at the very bottom of the document is [exception handling](https://requests.readthedocs.io/en/latest/user/quickstart/#errors-and-exceptions) mentioned at all.

```python
try:
    response = requests.get("http://example.com/")
except requests.exceptions.RequestException as err:
    # handle error
# ...
```

The exception flow really puts the burden on the user to understand what parts of the code can fail and how.

### Golang

Here's the equivalent code in Go, with an example taken directly from the official [http library](https://pkg.go.dev/net/http). Note that the example includes error handling, which I appreciate and I think is helpful towards teaching best practices. 
```go
resp, err := http.Get("http://example.com/")
if err != nil {
	// handle error
}
defer resp.Body.Close()
body, err := io.ReadAll(resp.Body)
// ...
```
In Go, an error is just another value. You are free to decide what to do with it, but I actually like that you are made aware that something could go wrong and that you should probably do something about it.

The downside is that Go code ends up with a lot of boilerplate like this:
```go
resp, err := http.Get("http://example.com/")
if err != nil {
	// handle error
}
defer resp.Body.Close()
body, err := io.ReadAll(resp.Body)
if err != nil {
    // handle error
}
data, err := DoSomeWork(body)
if err != nil {
    // handle error
}
```
Unfortunately, this boilerplate seems to be working as intended, as proposals to reduce this do not have a lot of support [[1](https://github.com/golang/go/issues/32437)], [[2](https://github.com/golang/go/issues/32811)], [[3](https://github.com/golang/go/issues/33233)].

### Bonus: C++
While C++ also has the try/catch syntax for exceptions, at work we typically use the [Abseil status library](https://abseil.io/docs/cpp/guides/status) instead. This works in a very similar way to Go, with errors being just another value. When using this library, functions that can fail return `absl::Status` (just the error) or `absl::StatusOr` (error or the value, if suceeded; C++ does not allow returning two values), so you are forced to handle errors gracefully.

Here's an example of how a code that handle errors typically could look like:

```c++
absl::StatusOr<http::Response> response = 
    request.get("http://test.com/test");
if (!response.ok()) {
    // bubble error further up
    return response.status();
}
// ...
```

Additionally C++ has helpful macros to reduce boilerplate that I miss in Go, such as [RETURN_IF_ERROR](https://github.com/protocolbuffers/protobuf/blob/main/src/google/protobuf/stubs/status_macros.h#L29) and [ASSIGN_OR_RETURN](https://github.com/protocolbuffers/protobuf/blob/main/src/google/protobuf/stubs/status_macros.h#L61). The code above could be reduced to the following:
```c++
ASSIGN_OR_RETURN(http::Response response, 
    request.get("http://test.com/test"));
// ...
```

Personally, I really prefer the flow where errors are just values and you have to explicitly do something about them, either bubbling them up or properly handling them. I find this makes it easier to write robust code, as you are aware of what could go wrong.

## Other goodies
Go is opinionated on how things should work, as we have seen with error handling above:
*  The [standard libraries](https://pkg.go.dev/std) cover a lot of use cases, including web development with the [http package](https://pkg.go.dev/net/http@go1.21.5), [image processing](https://pkg.go.dev/image@go1.21.5) and even a [SQL](https://pkg.go.dev/database/sql@go1.21.5) package to provide a generic interface for SQL databases.
*  Test files end with `_test.go`. 
*  [Formatter](https://go.dev/blog/gofmt) sorts imports alphabetically, puts braces at the end of the lines and just overall reduce the potential for debates about spacing or where to put braces.

## Last remarks
So far I've actually been having fun learning Go! The system I'm designing is a great opportunity to practice both design and learning a new language. I'm motivated to make the code very well tested, including integration tests. I've been in the flow multiple times and just looking forward to having time to work on it more. I'm also excited to see how easy this will be to deploy on the cloud.