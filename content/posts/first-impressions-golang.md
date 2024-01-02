---
title: "Learning Golang: first impressions"
date: 2024-01-02T00:45:28+02:00
draft: false
limmat_temperature: 7.0
images: ["images/golang_logo.jpg"]
tags: ["Tech"]
---

I'm working on a new project to collect hotel booking data regularly, scraping the Booking.com API. The idea is to deploy the scraper using [Google Cloud Functions](https://cloud.google.com/functions) and setup daily scraping using the [Google Cloud Scheduler](https://cloud.google.com/scheduler). I'm writing data to [BigQuery](https://cloud.google.com/bigquery) and plan on using this data later to train a machine learning model to predict hotel room prices.

I started with Python, but got frustrated with the development tools and decided to try something new instead. I've now been learning [Go](https://go.dev/), also known as Golang and I've been quite happy with it so far.

{{< figure src="/images/golang_logo.jpg" width="50%">}}

## Development environment
Go has excellent [integration](https://code.visualstudio.com/docs/languages/go) with [Visual Studio Code](https://code.visualstudio.com/), which is what I use to program. Everything just works:
*  IntelliSense: autocompletion of code snippets.
*  Automatic, sane code formatter. Code gets formatted automatically upon save. This was really a pain point for me in Python, as even changing how many spaces are used in indentation is non-trivial.
*  Blazing fast compilation. Code gets compiled as its being written, highlighting errors automatically, which is very helpful for me as I'm a beginner in Go.
*  Writing code that uses libraries automatically adds them to import declaration, it's magical. If I write `fmt.Println`, the editor automatically adds `import "fmt"`.

## Dependency management
Go has great dependency management and this is a massive advantage of Go compared to other languages. In Python, dependency management is typically done using [pip](https://pypi.org/project/pip/) and [virtualenv](https://virtualenv.pypa.io/en/latest/). Virtualenv creates isolated Python environments, as otherwise when you install a package using `pip` you are installing it for all projects. So the typical flow involves having a Virtualenv for every project you have, then configuring your code editor to also use that specific environment. Deployment involves sharing a `requirements.txt`, which contains all packages installed, typically created with `pip freeze`.

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
Unfortunately, this boilerplate seems to be working as intended according, as proposals to reduce this do not have a lot of support [[1](https://github.com/golang/go/issues/32437)], [[2](https://github.com/golang/go/issues/32811)], [[3](https://github.com/golang/go/issues/33233)].

### Bonus: C++
While C++ also has the try/catch syntax for exceptions, at work we typically use the [Abseil status library](https://abseil.io/docs/cpp/guides/status) instead. This works in a very similar way to Go, with errors being just another value. When using this library, functions that can fail return `absl::Status` (just the error) or `absl::StatusOr` (error or the value, if suceeded; C++ does not allow returning two values), so you are forced to handle errors gracefully.

Here's an example of how a code that handle errors typically could look like:

```c++
absl::StatusOr<http::Response> response = 
    request.get("http://test.com/test");
if (!response.ok()) {
    // handle error
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