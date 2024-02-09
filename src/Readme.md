In this project we are going to see   is works on the theory of  async await .

# Async await 
In Rust, async and await are keywords used to work with asynchronous programming, enabling you to write code that can execute concurrently without blocking the main thread. Here's a brief overview:

async: This keyword is used to define a block of code that can be executed asynchronously. It marks the beginning of an asynchronous function or block. When you mark a function as async, it means that the function can perform its operations asynchronously and return a special type called a "future" which represents the result of the computation.

await: This keyword is used within an async function to pause the execution of the function until the awaited future completes. When you await a future, the function will suspend its execution and return control to the caller until the awaited future produces a result. This allows you to write asynchronous code in a more sequential and readable manner.


# Working of this program
This code is a straightforward Rust programme that uses the reqwest crate, a well-liked HTTP client library in Rust, to send an HTTP GET request. The programme illustrates error handling with the error_chain crate and asynchronous programming with the async and await keywords.
# Here is the breakdown of code 


Let's break down the code:

```rust
use error_chain::error_chain;

error_chain! {
    foreign_links {
        Io(std::io::Error);
        HttpRequest(reqwest::Error);
    }
}
```

- `error_chain!`: This macro is used to define custom error types and automatically generate error handling code. In this case, it's creating an error type called `Error` with variants for I/O errors (`Io`) and HTTP request errors (`HttpRequest`). The `foreign_links` section specifies external errors that should be treated as part of the custom error type.

```rust
#[tokio::main]
async fn main() -> Result<()> {
    let res = reqwest::get("http://httpbin.org/get").await?;
    println!("Status: {}", res.status());
    println!("Headers:\n{:#?}", res.headers());
    let body = res.text().await?;
    println!("Body:\n{}", body);
    Ok(())
}
```

- `#[tokio::main]`: This attribute macro marks the `main` function to be executed within the Tokio runtime, enabling asynchronous execution.
- `async fn main() -> Result<()>`: This defines the `main` function as asynchronous, indicating that it can perform asynchronous operations and may return a `Result` type.
- `let res = reqwest::get("http://httpbin.org/get").await?;`: This line sends an asynchronous HTTP GET request to "http://httpbin.org/get" using the `reqwest::get` function. The `await` keyword is used to wait for the result of the asynchronous operation. If an error occurs during the request, it will be returned and handled using the `?` operator, which propagates the error up the call stack.
- `println!("Status: {}", res.status());`: This line prints the status code of the HTTP response.
- `println!("Headers:\n{:#?}", res.headers());`: This line prints the headers of the HTTP response in a formatted way.
- `let body = res.text().await?;`: This line asynchronously retrieves the body of the HTTP response as a string using the `text()` method provided by reqwest. Any error that occurs during this operation is propagated up the call stack.
- `println!("Body:\n{}", body);`: This line prints the body of the HTTP response.
- `Ok(())`: Finally, `Ok(())` is returned to indicate that the program executed successfully.

Overall, this program demonstrates how to perform asynchronous HTTP requests in Rust using the reqwest crate and handle errors using the error_chain crate.
