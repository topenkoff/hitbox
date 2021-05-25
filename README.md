# hitbox

[![Build status](https://github.com/hit-box/hitbox/actions/workflows/CI.yml/badge.svg)](https://github.com/hit-box/hitbox/actions?query=workflow)
[![Coverage Status](https://codecov.io/gh/hit-box/hitbox/branch/master/graph/badge.svg?token=tgAm8OBLkY)](https://codecov.io/gh/hit-box/hitbox)

 Hitbox is an asynchronous caching framework supporting multiple backends and suitable
 for distributed and for single-machine applications.

 ## Features
 - [x] Automatic cache key generation.
 - [x] Framework integrations:
     - [x] Actix ([hitbox-actix])
     - [ ] Actix-Web
 - [x] Multiple cache backend implementations:
     - [x] [RedisBackend]
     - [ ] In-memory backend
 - [x] Stale cache mechanics.
 - [ ] Cache locks for [dogpile effect] preventions.
 - [ ] Distributed cache locks.
 - [ ] Detailed metrics out of the box.

 ## Feature flags
 * derive - Support for [Cacheable] trait derive macros.
 * metrics - Support for metrics.

 ## Restrictions
 Default cache key implementation based on serde_qs crate
 and have some [restrictions](https://docs.rs/serde_qs/latest/serde_qs/#supported-types).

## Example

Dependencies:

```toml
[dependencies]
hitbox = "0.1"
```

Code:

> **_NOTE:_** Default cache key implementation based on serde_qs crate
> and have some [restrictions](https://docs.rs/serde_qs/latest/serde_qs/#supported-types).

## Example
First of all, you should derive [Cacheable] trait for your struct or enum:

```rust
use hitbox::prelude::*; // With features=["derive"]
use serde::{Deserialize, Serialize};

#[derive(Cacheable, Serialize)]
struct Ping {
    id: i32,
}
```
Or implement that trait manually:

```rust
# use hitbox::{Cacheable, CacheError};
# struct Ping { id: i32 }
impl Cacheable for Ping {
    fn cache_key(&self) -> Result<String, CacheError> {
        Ok(format!("{}::{}", self.cache_key_prefix(), self.id))
    }

    fn cache_key_prefix(&self) -> String { "Ping".to_owned() }
}
```

[Cacheable]: cache/trait.Cacheable.html
[CacheableResponse]: response/trait.CacheableResponse.html
[Backend]: ../hitbox_backend/trait.Backend.html
[RedisBackend]: ../hitbox_redis/actor/struct.RedisActor.html
[hitbox-actix]: ../hitbox_actix/index.html
[dogpile effect]: https://www.sobstel.org/blog/preventing-dogpile-effect/
 
