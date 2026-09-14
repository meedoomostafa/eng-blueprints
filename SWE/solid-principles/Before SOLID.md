Before we study SOLID Principles, we need to think about some fundamental questions to understand why we actually need them in the first place:

# Foundational Questions

1. **What makes a design good or bad?**
    
2. **How much existing code does a single requirement change force me to touch?**
    
3. **If I change one module, how likely is it to break an unrelated module?**
    
4. **Can I test this business logic in isolation without spinning up real databases or network calls?**
    

These questions lead us directly to the foundational concepts of software design. Let's break them down one by one.

# 1. Cohesion vs. Coupling

## Cohesion

Cohesion asks a very important question:

> **"How strongly do the responsibilities inside a single module belong together?"**

Look at this example:

```
class OrderCalculator
{
    CalculateSubtotal();
    CalculateTax();
    CalculateDiscount();
}
```

_How strongly are these inner methods related?_

They are highly related and focused on a single concern (pricing/calculations). This is **High Cohesion**.

Now look at this example:

```
class OrderService
{
    CalculatePrice();
    SaveToDatabase();
    SendEmail();
    GeneratePdf();
    CompressImage();
}
```

_Same question: How strongly are these methods related?_

Hardly at all! Why should an `OrderService` know how to compress images or construct PDF binaries? This is **Low Cohesion**.

## Coupling

Coupling asks another critical question:

> **"How dependent is this module on other modules to get its job done?"**

All non-trivial software requires components to talk to each other. Coupling is the spectrum of that relationship, ranging from **Loose** to **Tight**.

### Tight Coupling

Tight Coupling happens when a class directly depends on the concrete implementation details of another class rather than an abstraction.

Look at this example:

```
public class OrderService
{
    private readonly EmailSender _emailSender;

    public OrderService()
    {
        // Tight Coupling: Instantiating a concrete class directly
        _emailSender = new EmailSender();
    }

    public void CompleteOrder()
    {
        // Business logic...
        _emailSender.SendEmail("Order completed!");
    }
}
```

**Why is this problematic?**

- **Hard to Test:** You cannot test `OrderService` in isolation. Running a unit test will attempt to make real network calls or fire real emails.
    
- **Rigid:** If you want to switch to `SmsSender` or add push notifications, you are forced to modify `OrderService`'s internal code.
    

##### Loose Coupling

Loose Coupling happens when a class interacts through abstractions (interfaces/contracts) rather than concrete classes.

```
public interface IMessageSender
{
    void Send(string message);
}

public class OrderService
{
    private readonly IMessageSender _messageSender;

    // Dependency Injection: Decoupled via abstraction
    public OrderService(IMessageSender messageSender)
    {
        _messageSender = messageSender;
    }

    public void CompleteOrder()
    {
        _messageSender.Send("Order completed!");
    }
}
```

Now, `OrderService` doesn't know or care whether the sender is SMTP, Twilio, or a test mock (`Substitute.For<IMessageSender>()`).

# 2. Change Amplification & Blast Radius

A direct consequence of Low Cohesion and Tight Coupling is **Change Amplification**.

> **Change Amplification:** When a single, simple requirement change forces you to make modifications across multiple files and layers.

## Real-World Scenario:

If order discount rules change, and that calculation logic is duplicated across `CheckoutService`, `InvoiceService`, and `PaymentService`, a single business rule update requires editing 3+ files (and their respective unit tests).

#### The Architectural Goal:

$$\text{High Cohesion} + \text{Loose Coupling} = \text{Minimal Blast Radius}$$

Keep business rules encapsulated inside the Domain Model (Entities, Value Objects, or Domain Services) so that a business change touches only one single place (Single Source of Truth).

# 3. The Big Takeaway (So Far)

- **Good Design:** High Cohesion + Loose Coupling (Changes are isolated, unit testing is frictionless).
    
- **Bad Design:** Low Cohesion + Tight Coupling (Changes cascade like dominoes, testing is brittle).
    

# 4. The Real Enemy: Volatility (Rate of Change)

Not all code changes at the same rate or for the same reasons.

If you look closely at any production system, you will find two completely different categories of code:

#### The Stable Core

- **What it is:** The fundamental concepts and entities of your business domain (e.g., `Customer`, `Order`, `Account`).
    
- **Why it's stable:** The core meaning of an "Order" rarely changes from one sprint to the next.
    

## The Volatile Boundaries

- **What it is:** The policies, external tools, and delivery mechanisms that change constantly.
    
- **Examples:**
    
    - **Discount and pricing campaigns** (modified every holiday season).
        
    - **Payment providers** (switching between Stripe, PayPal, or Paymob).
        
    - **Notification channels** (moving from SMTP to SendGrid, or adding WhatsApp/SMS).
        
    - **Serialization formats** (changing JSON structures, DTOs, or third-party webhooks).
    
#### The Golden Rule:

> **Good design isolates volatile details from the stable core, and separates things that change for different reasons.**

If an update to your SMS provider forces you to modify your `Order` entity or core domain logic, your architectural boundaries have leaked.

# 5. Abstraction vs. Implementation

We just saw that abstractions help us achieve Loose Coupling. But here comes one of the biggest traps in modern software development:

> **Abstraction is not automatically good.**

Look at this example:

```
public interface ICalculator
{
    int Add(int x, int y);
}

public class Calculator : ICalculator
{
    public int Add(int x, int y) => x + y;
}
```

Ask yourself:

- _Will math addition ever change its implementation?_ No.
    
- _Do you have multiple polymorphic behaviors at runtime?_ No.
    
- _Does `Add(2, 3)` make network or disk calls that need mocking in unit tests?_ No.
    

Creating `ICalculator` here is **accidental complexity**. It adds zero architectural value, creates file bloat, and forces developers to jump through extra layers for no reason.

## Key Insight:

- **Adding interfaces everywhere** $\neq$ **good architecture.**
    
- Use abstractions only where there is **volatility**, a need for **runtime polymorphism**, or an **I/O testing boundary** (database, network, file system).
    

# 6. SOLID Are Heuristics, Not Dogmatic Rules

Before we examine the letters $S, O, L, I, D$, keep this rule in mind:

> **SOLID principles are heuristics (practical rules of thumb), not absolute dogmatic laws.**

Avoid these common misconceptions:

- ❌ _"If a class has more than one method, it violates SRP."_
    
- ❌ _"If a method is longer than 10 lines, it violates SRP."_
    
- ❌ _"If you write an `if/else` or `switch` statement, it violates OCP."_
    
- ❌ _"Every class must have an interface."_
    

#### Example:

```
public class User
{
    public string Name { get; set; }
    public string Email { get; set; }

    public bool HasValidEmail() => Email.Contains("@");
}
```

Does `HasValidEmail()` violate SRP?

**No!** Validating the internal sanity of its own properties is an intrinsic part of the `User` abstraction.

Now compare it to this:

```
public class User
{
    public string Name { get; set; }
    public string Email { get; set; }

    public void SaveToSqlServer() { ... }
    public void SendWelcomeEmail() { ... }
    public byte[] GenerateUserPdf() { ... }
}
```

Here, the class is directly tied to **three independent forces of change** (Database, Email Server, Document Rendering). This is an unmistakable SRP violation.

# 7. SOLID as a Design Vocabulary (The Interview Mindset)

In a code review or system design interview, never start by asking:

> _"Which letter of SOLID does this code break?"_

Instead, use this step-by-step diagnostic sequence:

```
What is changing?
       │
       ▼
Why is it changing?
       │
       ▼
Who or what drives that change? (Actors / Stakeholders)
       │
       ▼
What parts of the system should remain stable?
       │
       ▼
Where should the boundary live?
       │
       ▼
What contract/abstraction sits between them?
       │
       ▼
What behaviors must remain substitutable?
```

When you reason through these questions first, the correct SOLID principle emerges naturally as the solution.