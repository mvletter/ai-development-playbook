# Patterns

> Copy-paste solutions. Use these, don't reinvent.

## How to use

**When building**: Check if a pattern exists before writing new code
**After solving**: Add reusable solutions here
**In code**: Add `// AI-CONTEXT: See docs/patterns.md#[category]-[name]`

## Anchor format

Use category-prefixed anchors from the start:
- `#api-response-format`
- `#data-transaction`
- `#error-validation`

This enables splitting later without changing code references.

## Categories

- **api** - Endpoints, responses, validation
- **data** - Database access, queries, transactions
- **error** - Catching, throwing, logging
- **auth** - Authentication, authorization
- **test** - Test patterns, mocks, fixtures
- **observability** - Logging, tracing, monitoring
- **resilience** - Error handling for external services and AI features

---

## observability-structured-logging

**When to use:** Any code that needs debugging in production, especially AI-generated code

AI lacks contextual understanding that makes debugging intuitive. Compensate with structured logging.

```csharp
public class StructuredLogger
{
    public void LogAgentAction(string action, object context)
    {
        var entry = new {
            timestamp = DateTime.UtcNow.ToString("o"),
            requestId = GetRequestId(),
            sessionId = GetSessionId(),
            action = action,
            context = context,
            reasoning = "why this action was taken"
        };
        Logger.Info(JsonSerializer.Serialize(entry));
    }
}
```

**Log these:** Agent decisions, tool calls, reasoning steps, business logic assumptions.

---

## error-propagation

**When to use:** Any error handling, especially in AI-generated code

AI tends to swallow errors with catch-all handlers. Always propagate errors up.

```csharp
public async Task<Result> ProcessOrder(Order order)
{
    // Validate early, fail fast
    if (order == null) throw new ArgumentNullException(nameof(order));
    if (!order.IsValid()) throw new ValidationException(order.Errors);
    
    try 
    {
        await _repository.Save(order);
        return Result.Success();
    }
    catch (Exception ex)
    {
        _logger.LogError(ex, "Failed to process order {Id}", order.Id);
        throw; // Log AND re-throw, never swallow
    }
}
```

**Rules:**
- Validate inputs with guard clauses at the start
- Log errors with context, then re-throw
- Never return success after catching an exception

**See also:** docs/pitfalls.md#error-catch-all

---

## resilience-ai-fail-loudly

**When to use:** Any AI-powered feature

Traditional software fails visibly (500 errors, crashes). AI fails invisibly - it produces confident, plausible-sounding answers that are wrong. Silent fallbacks make this worse: users don't know if they're seeing real AI output, cached data, or a template.

**Don't do this:**
```csharp
// BAD: Silent fallback hides AI failure
try {
    return await _aiService.Process(request);
} catch {
    return await _cache.GetStale(request.Key); // User thinks this is AI output
}
```

**Do this instead:**
```csharp
public async Task<Response> GetAIResponse(Request request)
{
    try 
    {
        var result = await _aiService.Process(request);
        return new Response { 
            Data = result,
            Source = ResponseSource.AI,
            GeneratedAt = DateTime.UtcNow
        };
    } 
    catch (Exception ex)
    {
        _logger.LogWarning(ex, "AI service failed for request {Id}", request.Id);
        
        // Explicit failure - user knows AI didn't work
        return new Response { 
            Data = null,
            Source = ResponseSource.Failed,
            Error = "AI feature temporarily unavailable",
            ShowFallbackUI = true
        };
    }
}
```

**Rule:** When AI fails, make it obvious. Users deserve to know what they're getting.

---

---

## Templates (remove when adding real entries)

### api-[name]

**When to use:** [Trigger condition]

```
[Code to copy-paste]
```

**See also:** docs/pitfalls.md#api-[related]

### data-[name]

...

---

## Adding new patterns

1. Add entry with category-prefixed anchor
2. Include copy-paste ready code
3. Link to related pitfalls
4. Add AI-CONTEXT comment in code that uses it

---

## Scaling

**When to split:** >30 patterns OR >10k tokens OR hard to navigate

**How to split:**

1. Create `docs/patterns/` directory
2. Move entries to category files: `api.md`, `data.md`, etc.
3. Keep this file as index (see below)
4. **Code references don't change** - anchors stay the same

**This file becomes index:**

```markdown
# Patterns Index

Categories:
- [api](patterns/api.md) - Endpoints, responses, validation
- [data](patterns/data.md) - Database access, queries
- [error](patterns/error.md) - Catching, throwing, logging

Anchor format: `#[category]-[name]`
Files location: `docs/patterns/[category].md`
```

**After split, AI-CONTEXT still works:**
```
// AI-CONTEXT: See docs/patterns.md#api-response-format
→ AI reads index, finds api.md, locates #response-format
```
