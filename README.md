# Blackman AI Rust SDK

Official Rust client for [Blackman AI](https://www.useblackman.ai) - The AI API proxy that optimizes token usage to reduce costs.

## Features

- 🚀 Drop-in replacement for OpenAI, Anthropic, and other LLM APIs
- 💰 Automatic token optimization (save 20-40% on costs)
- 📊 Built-in analytics and cost tracking
- 🔒 Enterprise-grade security with SSO support
- ⚡ Low latency overhead (<50ms)
- 🎯 Semantic caching for repeated queries
- ⚙️ Async/await with Tokio
- 🦀 Strongly typed with Serde

## Installation

Add to your `Cargo.toml`:

```toml
[dependencies]
blackman-client = "0.0.9"
tokio = { version = "1.0", features = ["full"] }
```

## Quick Start

```rust
use blackman_client::{apis, models};

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    // Configure client
    let config = apis::configuration::Configuration {
        base_path: "https://app.useblackman.ai".to_string(),
        bearer_access_token: Some("sk_your_blackman_api_key".to_string()),
        ..Default::default()
    };

    // Create completion request
    let request = models::CompletionRequest {
        provider: "OpenAI".to_string(),
        model: "gpt-4o".to_string(),
        messages: vec![
            models::Message {
                role: "user".to_string(),
                content: "Explain quantum computing in simple terms".to_string(),
            }
        ],
        ..Default::default()
    };

    // Send request
    let response = apis::completions_api::completions(&config, request).await?;

    println!("{}", response.choices[0].message.content);
    println!("Tokens used: {}", response.usage.total_tokens);

    Ok(())
}
```

## Authentication

Get your API key from the [Blackman AI Dashboard](https://app.useblackman.ai/settings/api-keys).

```rust
let config = apis::configuration::Configuration {
    base_path: "https://app.useblackman.ai".to_string(),
    bearer_access_token: Some("sk_your_blackman_api_key".to_string()),
    ..Default::default()
};
```

## Framework Integration

### Axum Web Framework

```rust
use axum::{
    extract::Json,
    response::IntoResponse,
    routing::post,
    Router,
};
use blackman_client::{apis, models};
use serde::{Deserialize, Serialize};
use std::sync::Arc;

#[derive(Clone)]
struct AppState {
    blackman_config: Arc<apis::configuration::Configuration>,
}

#[derive(Deserialize)]
struct ChatRequest {
    message: String,
}

#[derive(Serialize)]
struct ChatResponse {
    response: String,
    tokens: i32,
}

async fn chat_handler(
    axum::extract::State(state): axum::extract::State<AppState>,
    Json(payload): Json<ChatRequest>,
) -> impl IntoResponse {
    let request = models::CompletionRequest {
        provider: "OpenAI".to_string(),
        model: "gpt-4o".to_string(),
        messages: vec![
            models::Message {
                role: "user".to_string(),
                content: payload.message,
            }
        ],
        ..Default::default()
    };

    match apis::completions_api::completions(&state.blackman_config, request).await {
        Ok(response) => {
            let chat_response = ChatResponse {
                response: response.choices[0].message.content.clone(),
                tokens: response.usage.total_tokens,
            };
            Json(chat_response).into_response()
        }
        Err(e) => {
            eprintln!("Error: {:?}", e);
            axum::http::StatusCode::INTERNAL_SERVER_ERROR.into_response()
        }
    }
}

#[tokio::main]
async fn main() {
    let config = Arc::new(apis::configuration::Configuration {
        base_path: "https://app.useblackman.ai".to_string(),
        bearer_access_token: Some(
            std::env::var("BLACKMAN_API_KEY").expect("BLACKMAN_API_KEY not set")
        ),
        ..Default::default()
    });

    let state = AppState {
        blackman_config: config,
    };

    let app = Router::new()
        .route("/chat", post(chat_handler))
        .with_state(state);

    let listener = tokio::net::TcpListener::bind("0.0.0.0:8080").await.unwrap();
    axum::serve(listener, app).await.unwrap();
}
```

### Actix Web Framework

```rust
use actix_web::{post, web, App, HttpResponse, HttpServer, Responder};
use blackman_client::{apis, models};
use serde::{Deserialize, Serialize};
use std::sync::Arc;

struct AppState {
    blackman_config: Arc<apis::configuration::Configuration>,
}

#[derive(Deserialize)]
struct ChatRequest {
    message: String,
}

#[derive(Serialize)]
struct ChatResponse {
    response: String,
}

#[post("/chat")]
async fn chat(
    data: web::Data<AppState>,
    req: web::Json<ChatRequest>,
) -> impl Responder {
    let request = models::CompletionRequest {
        provider: "OpenAI".to_string(),
        model: "gpt-4o".to_string(),
        messages: vec![
            models::Message {
                role: "user".to_string(),
                content: req.message.clone(),
            }
        ],
        ..Default::default()
    };

    match apis::completions_api::completions(&data.blackman_config, request).await {
        Ok(response) => {
            let chat_response = ChatResponse {
                response: response.choices[0].message.content.clone(),
            };
            HttpResponse::Ok().json(chat_response)
        }
        Err(e) => {
            eprintln!("Error: {:?}", e);
            HttpResponse::InternalServerError().finish()
        }
    }
}

#[actix_web::main]
async fn main() -> std::io::Result<()> {
    let config = Arc::new(apis::configuration::Configuration {
        base_path: "https://app.useblackman.ai".to_string(),
        bearer_access_token: Some(
            std::env::var("BLACKMAN_API_KEY").expect("BLACKMAN_API_KEY not set")
        ),
        ..Default::default()
    });

    let state = web::Data::new(AppState {
        blackman_config: config,
    });

    HttpServer::new(move || {
        App::new()
            .app_data(state.clone())
            .service(chat)
    })
    .bind("0.0.0.0:8080")?
    .run()
    .await
}
```

## Advanced Usage

### Custom HTTP Client

```rust
use reqwest::Client;
use std::time::Duration;

let client = Client::builder()
    .timeout(Duration::from_secs(60))
    .build()?;

let config = apis::configuration::Configuration {
    base_path: "https://app.useblackman.ai".to_string(),
    bearer_access_token: Some("sk_your_blackman_api_key".to_string()),
    client,
    ..Default::default()
};
```

### Error Handling

```rust
match apis::completions_api::completions(&config, request).await {
    Ok(response) => {
        println!("{}", response.choices[0].message.content);
    }
    Err(e) => {
        eprintln!("API Error: {:?}", e);
        // Handle specific error types
        match e {
            apis::Error::Reqwest(e) => eprintln!("Network error: {}", e),
            apis::Error::Serde(e) => eprintln!("Serialization error: {}", e),
            apis::Error::Io(e) => eprintln!("IO error: {}", e),
            _ => eprintln!("Unknown error"),
        }
    }
}
```

### Retry Logic with Backoff

```rust
use tokio::time::{sleep, Duration};

async fn completions_with_retry(
    config: &apis::configuration::Configuration,
    request: models::CompletionRequest,
    max_retries: u32,
) -> Result<models::CompletionResponse, apis::Error> {
    let mut retries = 0;
    let mut delay = Duration::from_millis(100);

    loop {
        match apis::completions_api::completions(config, request.clone()).await {
            Ok(response) => return Ok(response),
            Err(e) if retries < max_retries => {
                eprintln!("Request failed, retrying in {:?}... (attempt {})", delay, retries + 1);
                sleep(delay).await;
                retries += 1;
                delay *= 2; // Exponential backoff
            }
            Err(e) => return Err(e),
        }
    }
}
```

### Concurrent Requests

```rust
use futures::future::join_all;

let tasks: Vec<_> = messages.iter().map(|msg| {
    let config = config.clone();
    let request = models::CompletionRequest {
        provider: "OpenAI".to_string(),
        model: "gpt-4o".to_string(),
        messages: vec![
            models::Message {
                role: "user".to_string(),
                content: msg.clone(),
            }
        ],
        ..Default::default()
    };

    tokio::spawn(async move {
        apis::completions_api::completions(&config, request).await
    })
}).collect();

let results = join_all(tasks).await;
```

## Documentation

- [Full API Reference](https://app.useblackman.ai/docs)
- [Getting Started Guide](https://app.useblackman.ai/docs/getting-started)
- [Rust Examples](https://github.com/blackman-ai/rust-sdk/tree/main/examples)
- [Docs.rs](https://docs.rs/blackman-client)

## Requirements

- Rust 1.70 or higher
- Tokio runtime for async operations

## Support

- 📧 Email: [support@blackman.ai](mailto:support@blackman.ai)
- 💬 Discord: [Join our community](https://discord.gg/blackman-ai)
- 🐛 Issues: [GitHub Issues](https://github.com/blackman-ai/rust-sdk/issues)

## License

MIT © Blackman AI
