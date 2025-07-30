# [Dependency Injection](https://learn.microsoft.com/en-us/dotnet/core/extensions/dependency-injection)

A dependency is an object that another object depends on. Dependency injection is a software design patthern which is a technique that achieves [Inversion of Control](https://learn.microsoft.com/en-us/dotnet/architecture/modern-web-apps-azure/architectural-principles#dependency-inversion). 

## Service Lifetime Options
.NET provides three main lifetimes when registering dependencies:

| Method              | Lifetime  | What it means |
| ------------------- | --------- | ------------- |
| `AddSingleton<T>()` |	Singleton |	One single instance for the entire application lifetime. Shared by all. |
| `AddTransient<T>()` |	Transient |	A new instance is created every time it's requested (injected). |
| `AddScoped<T>()`    |	Scoped    |	A new instance per scope — used in web apps per HTTP request. Not super relevant in console apps unless you create custom scopes manually. |
