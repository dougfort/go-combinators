# The Combinator Pattern in Go

I see a lot of the [combinator pattern](https://wiki.haskell.org/Combinator_pattern) in Rust and other languages.

```rust
    let addr = matches.value_of("address")
        .map(|s| s.to_owned())
        .or(env::var("ADDRESS").ok())
        .unwrap_or_else(|| "127.0.0.1:8080".into())
        .parse()
        .expect("can't parse ADDRESS variable");
```

It crops up occasionally in go, for example in [zerolog](https://github.com/rs/zerolog)

```golang
    logger.Info().
        Str("XDS_SERVER_ADDRESS", viper.GetString("xds_server_address")).
        Dur("XDS_POLLING_INTERVAL", viper.GetDuration("xds_polling_interval")).
        Dur("XDS_MAX_SILENCE", viper.GetDuration("xds_max_silence")).
        Dur("XDS_HTTP_CLIENT_TIMEOUT", viper.GetDuration("xds_http_client_timeout")).
        Msg("using xds")
```

At [Decipher](http://deciphernow.com/), we make a lot of use of [functional options](https://dave.cheney.net/2014/10/17/functional-options-for-friendly-apis) for a similar purpose.

I think we can use combinators, along with functional options to reduce the occurrence of go functions which take a confusing number of parameters.
