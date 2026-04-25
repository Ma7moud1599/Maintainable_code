# Maintainable Code — Laravel Design Patterns

A Laravel project demonstrating how to write clean, maintainable, and testable backend code using SOLID principles and design patterns. Focuses on a payment processing domain as a practical example.

## What This Demonstrates

- **Interface segregation** — `PaymentOption` and `PaymentProcessor` contracts decouple consumers from implementations
- **Factory pattern** — `PaymentOptionsFactory` selects the right processor at runtime without conditionals scattered in controllers
- **Dependency injection** — controllers receive contracts, not concrete classes
- **Testability** — `TestablePayment` class shows how to design for unit testing without mocking internals
- **Single Responsibility** — `PaymentController` handles HTTP, `PaymentProcessingController` handles orchestration, `BankApi` handles external calls

## Domain: Payment Processing

The project models a payment system that supports multiple processors (Payoneer, Wire Transfer, Wise) through a common interface.

## Project Structure

```
app/
├── Classes/
│   ├── Payment.php              # Core payment entity
│   ├── TestablePayment.php      # Variant designed for unit testing
│   ├── BankApi.php              # External bank API wrapper
│   ├── User.php
│   └── WireProcessor.php        # Wire transfer implementation
├── Contracts/
│   ├── PaymentOption.php             # Interface: what options are available
│   ├── PaymentProcessor.php          # Interface: how to process
│   ├── PayoneerRepositoryInterface.php
│   ├── WireRepositoryInterface.php
│   └── WiseRepositoryInterface.php
├── Factories/
│   └── PaymentOptionsFactory.php    # Resolves processor from request context
└── Http/Controllers/
    ├── PaymentController.php          # HTTP: show payment form, handle input
    └── PaymentProcessingController.php # Orchestrates payment flow
```

## Key Patterns Applied

### Interface + Factory
```
Client → PaymentController → PaymentOptionsFactory → (Payoneer|Wire|Wise)Processor
                                                          ↕
                                                    PaymentProcessor contract
```

### Contract-driven Development
Every processor implements `PaymentProcessor`. Controllers only depend on the contract — swapping implementations requires zero controller changes.

## Installation

```bash
git clone https://github.com/Ma7moud1599/Maintainable_code.git
cd Maintainable_code

composer install
cp .env.example .env
php artisan key:generate

php artisan migrate
php artisan serve
```

## License

MIT
