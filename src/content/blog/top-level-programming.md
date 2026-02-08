---
title: "Top-Level Programming in C#"
description: "A complete, practical course on C# top-level programming: before vs after, rules, pitfalls, real-world scenarios, and a few jokes to survive Program.cs."
pubDate: "Feb 08 2026"
heroImage: "../../assets/top-level-programming-csharp.jpg"
---

## Chapter 0 — The problem Microsoft was tired of

Before C# 9, every beginner wrote this:

```csharp
using System;

class Program
{
    static void Main(string[] args)
    {
        Console.WriteLine("Hello World");
    }
}
```

Microsoft looked at this and said:

> “So… 6 lines of ceremony to print one sentence?
> Java already does this. Let’s not be Java.”

And thus… **Top-Level Programming** was born.

---

## Chapter 1 — What is Top-Level Programming (no marketing lies)

**Definition (real one):**
Top-level programming allows you to write executable statements **without explicitly defining `Program` and `Main()`**.

**What it is NOT:**

* Not a new language
* Not scripting
* Not slower
* Not magic (it’s compiler sugar)

**What it IS:**

* Less boilerplate
* Same runtime behavior
* Same IL code

---

## Chapter 2 — Before vs After (the shock)

### Before (classic C#)

```csharp
class Program
{
    static void Main()
    {
        Console.WriteLine("Hello");
    }
}
```

### After (top-level)

```csharp
Console.WriteLine("Hello");
```

That’s it.

Yes, C# finally let you breathe.

---

## Chapter 3 — The invisible lie (what the compiler really does)

Your top-level code:

```csharp
Console.WriteLine("Hi");
```

Becomes (roughly):

```csharp
internal class Program
{
    static void Main(string[] args)
    {
        Console.WriteLine("Hi");
    }
}
```

So if someone says:

> “Top-level has no Main”

They are wrong.
It **has one**, you just don’t babysit it anymore.

---

## Chapter 4 — The ONE RULE that ruins people’s day

### 🔥 Rule of Doom

> **All top-level statements MUST come before any type declaration**

### This works:

```csharp
Console.WriteLine("Start");

class User {}
```

### This explodes:

```csharp
class User {}

Console.WriteLine("Start"); // 💥
```

Compiler error translation:

> “I already saw a class, don’t try to sneak executable code now.”

---

## Chapter 5 — Functions (aka “why static suddenly disappeared”)

### Before

```csharp
class Program
{
    static void Main()
    {
        Console.WriteLine(Add(2, 3));
    }

    static int Add(int a, int b)
    {
        return a + b;
    }
}
```

### After

```csharp
Console.WriteLine(Add(2, 3));

int Add(int a, int b)
{
    return a + b;
}
```

### Important truths

* This is a **local function**
* `static` is **forbidden**
* Order **does not matter**

### This works:

```csharp
Console.WriteLine(Add(1, 2));

int Add(int a, int b) => a + b;
```

### This does NOT:

```csharp
static int Add(int a, int b) => a + b;
```

Why?
Because you’re already inside a static `Main`.
Adding `static` again is like putting a hat on a hat.

---

## Chapter 6 — Variables (no, they are NOT hoisted)

### Works

```csharp
int x = 10;
Console.WriteLine(x);
```

### Fails

```csharp
Console.WriteLine(x);
int x = 10;
```

Why?
C# is not JavaScript.
Variables don’t teleport from the future.

---

## Chapter 7 — Multiple files (only ONE boss file)

### This is illegal

```csharp
// File1.cs
Console.WriteLine("A");

// File2.cs
Console.WriteLine("B");
```

Error meaning:

> “Two Main methods walked into a bar. I rejected both.”

### Correct structure

```csharp
// Program.cs
Console.WriteLine("Hello");

// User.cs
class User {}
```

Only **one file** gets to be special.

---

## Chapter 8 — `using` directives (order matters, don’t freestyle)

### Correct

```csharp
using System;

Console.WriteLine("Hello");
```

### Wrong

```csharp
Console.WriteLine("Hello");
using System; // ❌
```

Compiler:

> “Imports go at the top. Always.”

### Bonus: Global usings

```csharp
global using System;
```

This is how .NET 6 projects stay clean.

---

## Chapter 9 — Async programs (C# finally caught up)

### Before

```csharp
class Program
{
    static async Task Main()
    {
        await Task.Delay(1000);
        Console.WriteLine("Done");
    }
}
```

### After

```csharp
await Task.Delay(1000);
Console.WriteLine("Done");
```

Compiler quietly switches `Main` to:

```csharp
static async Task Main(string[] args)
```

No ceremony. No drama.

---

## Chapter 10 — Return, exit, and “why can’t I return 0?”

### This works

```csharp
return;
```

### This does NOT

```csharp
return 0;
```

Why?
Your implicit `Main` returns `void` or `Task`, not `int`.

### Exit codes (correct way)

```csharp
Environment.Exit(0);
```

---

## Chapter 11 — ASP.NET Core (why this exists at all)

### Before (.NET 5)

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }
}
```

### After (.NET 6+)

```csharp
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.MapGet("/", () => "Hello");

app.Run();
```

This is why top-level programming was **really introduced**.
Not for console apps. For **clean startup pipelines**.

---