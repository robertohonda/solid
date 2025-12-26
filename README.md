# SOLID

SOLID is a set of five software design principles that help create code that is maintainable, extensible, testable, and scalable.

- **S — Single Responsibility Principle (SRP)**
- **O — Open / Closed Principle (OCP)**
- **L — Liskov Substitution Principle**
- **I — Interface Segregation Principle**
- **D — Dependency Inversion Principle**

## Single Responsibility Principle (SRP)
A module, class, or function should have only one task or responsibility.

❌ Violating SRP
```Ts
class UserService {
  createUser(user: { name: string; email: string }) {
    // business logic
  }

  saveToDatabase(user: { name: string; email: string }) {
    // database logic
  }

  sendWelcomeEmail(user: { name: string; email: string }) {
    // email logic
  }
}
```

✅ Following SRP
```TS
type User = {
  name: string;
  email: string;
};

class UserService {
  createUser(user: User) {
    // business logic
  }
}

class UserRepository {
  save(user: User) {
    // database logic
  }
}

class EmailService {
  sendWelcomeEmail(user: User) {
    // email logic
  }
}
```

## Open / Closed Principle (OCP)
software entities (classes, modules, functions, etc.) should be open for extension, but closed for modification.

❌ Violating OCP
```TS
type PaymentMethod = 'card' | 'paypal';

class PaymentService {
  pay(amount: number, method: PaymentMethod) {
    if (method === 'card') {
      // card payment logic
    } else if (method === 'paypal') {
      // paypal payment logic
    }
  }
}
```
Every new payment method requires modifying the above class.

✅ Following OCP
```TS
interface PaymentStrategy {
  pay(amount: number): void;
}

class CardPayment implements PaymentStrategy {
  pay(amount: number) {
    // card payment logic
  }
}

class PaypalPayment implements PaymentStrategy {
  pay(amount: number) {
    // paypal payment logic
  }
}

class PaymentService {
  constructor(private paymentStrategy: PaymentStrategy) {}

  pay(amount: number) {
    this.paymentStrategy.pay(amount);
  }
}
```
