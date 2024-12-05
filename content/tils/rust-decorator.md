+++
title = "Rust Decorator"
date = 2024-11-06
type = "til"
description = "Learning about decorater "
in_search_index = true
[taxonomies]
tags = ["rust", "til"]
+++

I wanted to add see if there were any options like a `python decorator` in rust to wrap functions and add run some function before and after a function, example wrapping a function with extra logs, like tracking time it took for execution. Though my original requirement was to use redis lock for certain functions

```rust

use std::future::Future;
use std::pin::Pin;
use std::task::{Context, Poll};
use std::time::Instant;

// Define `add_logging` to accept an async function
fn add_logging<F, Fut>(f: F) -> impl Fn() -> Pin<Box<dyn Future<Output = ()>>>
where
    F: Fn() -> Fut + Send + Sync + 'static,
    Fut: Future<Output = ()> + Send + 'static,
{
    move || {
        Box::pin(async move {
            println!("Before async function call");
            let start = Instant::now();

            f().await;

            let duration = start.elapsed();
            println!("After async function call. Duration: {:?}", duration);
        })
    }
}

async fn my_function() {
    println!("This is my async function!");
}

#[tokio::main] // Or `async-std::main` depending on your runtime
async fn main() {
    let decorated_function = add_logging(my_function);
    decorated_function().await; // Calls the wrapped async function
}
```

Now the issue here is we can't pass arguments to the above function which is being wrapped as it is a `Fn()`. and the way to pass functions dynamically is using something as follows..

```rust

use std::future::Future;
use std::pin::Pin;
use std::time::Instant;

// Define `add_logging` to accept a function with dynamic parameters
fn add_logging<F, Fut, Args>(f: F) -> impl Fn(Args) -> Pin<Box<dyn Future<Output = Fut::Output> + Send>>
where
    F: Fn(Args) -> Fut + Send + Sync + 'static,
    Fut: Future + Send + 'static,
    Args: Send + 'static,
{
    move |args: Args| {
        Box::pin(async move {
            println!("Before async function call");
            let start = Instant::now();

            let result = f(args).await;

            let duration = start.elapsed();
            println!("After async function call. Duration: {:?}", duration);

            result // Return the result of the wrapped function
        })
    }
}

// Example async function with parameters
async fn my_function(input: String, count: usize) -> String {
    println!("Inside my_function with input: {}, count: {}", input, count);
    format!("Processed: {} (count: {})", input, count)
}

#[tokio::main]
async fn main() {
    // Decorate the async function
    let decorated_function = add_logging(|(input, count): (String, usize)| my_function(input, count));

    // Call the decorated function with dynamic parameters
    let result = decorated_function(("Hello, world!".to_string(), 42)).await;

    println!("Result: {}", result);
}
```
