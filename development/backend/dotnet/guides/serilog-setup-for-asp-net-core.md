# Serilog Setup Guide for ASP.NET Core

## Overview
This guide documents the how to add and configure Serilog into an ASP.NET Core project.

## Packages
- `Serilog.AspNetCore` (version 9.0.0) - Core Serilog integration with ASP.NET Core
- `Serilog.Sinks.Console` (version 6.0.0) - Console output sink
- `Serilog.Sinks.File` (version 7.0.0) - File output sink

## Configuration Files

### appsettings.json
Add the following Serilog configuration section:
```json
"Serilog": {
    "Using": ["Serilog.Sinks.Console", "Serilog.Sinks.File"],
    "MinimumLevel": {
        "Default": "Information",
        "Override": {
            "Microsoft": "Warning",
            "System": "Warning",
            "Hangfire": "Information"
        }
    },
    "WriteTo": [
        {
            "Name": "Console",
            "Args": {
                "outputTemplate": "[{Timestamp:yyyy-MM-dd HH:mm:ss.fff zzz}] [{Level:u3}] {Message:lj}{NewLine}{Exception}"
            }
        },
        {
            "Name": "File",
            "Args": {
                "path": "logs/your-api-.log",
                "rollingInterval": "Day",
                "retainedFileCountLimit": 30,
                "outputTemplate": "[{Timestamp:yyyy-MM-dd HH:mm:ss.fff zzz}] [{Level:u3}] [{SourceContext}] {Message:lj}{NewLine}{Exception}",
                "fileSizeLimitBytes": 10485760,
                "rollOnFileSizeLimit": true
            }
        }
    ],
    "Enrich": ["FromLogContext", "WithMachineName", "WithThreadId"]
}
```
## Program.cs Setup

### Add Using Statement
```csharp
using Serilog;
```

### Add Serilog Configuration
```csharp
// Configure Serilog
Log.Logger = new LoggerConfiguration()
    .ReadFrom.Configuration(builder.Configuration)
    .CreateLogger();

builder.Host.UseSerilog();

try
{
    Log.Information("Starting ASP.NET Core API");

    // ... rest of your application code
}
catch (Exception ex)
{
    Log.Fatal(ex, "Application terminated unexpectedly");
}
finally
{
    Log.CloseAndFlush();
}
```

### Add Request Logging
```csharp
app.UseSerilogRequestLogging();
```

## Log File Configuration

### File Rotation
- **Daily rotation**: New file created each day
- **Size-based rotation**: Files are rotated when they exceed 10MB
- **Retention**: 7 days

### Output Format
```
[2025-08-28 14:30:45.123 +02:00] [INF] [YourProject] Starting some code execution at 08/28/2025 12:30:45
```

## Logging Features

### Log Levels
- **Debug**: Detailed information for debugging (Development only)
- **Information**: General application information
- **Warning**: Warning messages for potential issues
- **Error**: Error messages with exceptions
- **Fatal**: Critical errors that cause application termination

### Enrichment
Logs are automatically enriched with:
- **Machine Name**: Identifies which server generated the log
- **Thread ID**: Useful for debugging concurrency issues
- **Source Context**: Shows which class generated the log message

### Request Logging
All HTTP requests are automatically logged with:
- Request method and path
- Response status code
- Response time
- User agent (if available)

## Monitoring and Troubleshooting

### Checking Logs
1. Navigate to the `logs` folder in your application directory
2. Open the latest log file for your environment
3. Use text search or log analysis tools to find specific events

### Log Analysis Tools
Consider using tools like:
- **Seq** (free for single user, paid for teams)
- **Elasticsearch + Kibana**
- **Grafana Loki**
- **Azure Log Analytics** (if deploying to Azure)

### Common Log Patterns
```csharp
// Performance monitoring
using IDisposable activity = _logger.BeginScope("Processing some data", data);
Stopwatch stopwatch = Stopwatch.StartNew();
// ... Do work
_logger.LogInformation("Completed processing in {ElapsedMs}ms", stopwatch.ElapsedMilliseconds);

// Error context
try
{
    // Risky operation
}
catch (Exception ex)
{
    _logger.LogError(ex, "Failed to process data with error: {ErrorMessage}", data, ex.Message);
    throw;
}
```

## Troubleshooting

### Issue: Logs Not Appearing
- Check that the `logs` directory has write permissions
- Verify Serilog configuration is valid JSON
- Check for startup errors in the console

### Issue: Too Many Logs
- Increase the minimum log level for noisy components
- Adjust the `Override` settings in configuration
- Consider using log filtering

### Issue: Performance Impact
- Reduce log levels in high-traffic areas
- Consider using asynchronous sinks for high-volume scenarios
- Monitor application performance metrics