# Application configuration

## Keep parsing at the boundary

Parse flags, environment variables, and files in the executable layer, then pass a validated configuration struct into constructors. Domain packages should not read process-global configuration.

```go
type Config struct {
    Address      string
    DatabaseURL  string
    ReadTimeout  time.Duration
    WriteTimeout time.Duration
}

func loadConfig() (Config, error) {
    cfg := Config{
        Address:      envOrDefault("APP_ADDRESS", ":8080"),
        DatabaseURL:  os.Getenv("APP_DATABASE_URL"),
        ReadTimeout:  5 * time.Second,
        WriteTimeout: 10 * time.Second,
    }
    if cfg.DatabaseURL == "" {
        return Config{}, errors.New("APP_DATABASE_URL is required")
    }
    return cfg, nil
}
```

## Precedence

If multiple sources are supported, document one deterministic precedence order. A common policy is explicit flags, environment variables, configuration file, then defaults.

## Validation

Validate configuration once during startup and fail with actionable messages. Keep secrets out of committed files and avoid printing secret values in startup logs.

## Testability

Separate source loading from validation. Test validation with ordinary values, and test environment parsing with `t.Setenv` so process state is restored automatically.
