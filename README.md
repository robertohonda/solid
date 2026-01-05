# SOLID

SOLID is a set of five software design principles that help create code that is maintainable, extensible, testable, and scalable.

- **S — Single Responsibility Principle (SRP)**
- **O — Open / Closed Principle (OCP)**
- **L — Liskov Substitution Principle (LSP)**
- **I — Interface Segregation Principle (ISP)**
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

Key idea: You extend behavior by adding new code, not by changing tested code.

## Liskov Substitution Principle (LSP)

Objects of a superclass should be **replaceable with objects of its subclasses** without breaking the correctness of the program.

In other words, a subclass should behave in a way that **does not violate the expectations** set by the parent class or interface.

❌ Violating LSP
```TS
class Employee {
  getBonus(): number {
    return 1000;
  }
}

class Intern extends Employee {
  getBonus(): number {
    throw new Error('Interns do not receive bonuses');
  }
}

function printBonus(employee: Employee) {
  console.log(employee.getBonus());
}

printBonus(new Intern()); // 💥 breaks at runtime
```

✅ Following LSP
```TS
interface Employee {
  getBonus(): number;
}

class FullTimeEmployee implements Employee {
  getBonus(): number {
    return 1000;
  }
}

class Contractor implements Employee {
  getBonus(): number {
    return 0;
  }
}

function printBonus(employee: Employee) {
  console.log(employee.getBonus());
}
```

## Interface Segregation Principle (ISP)
Clients should not be forced to depend on interfaces they **do not use**.

In simple terms: Prefer **small, specific interfaces** over large, general-purpose ones.

**Scenario:**  
Different employees have different responsibilities in a company system.

❌ Violating ISP
```TS
interface Employee {
  work(): void;
  manage(): void;
  writeCode(): void;
}

class Developer implements Employee {
  work() {}
  manage() {
    throw new Error('Developers do not manage');
  }
  writeCode() {}
}

class Manager implements Employee {
  work() {}
  manage() {}
  writeCode() {
    throw new Error('Managers do not write code');
  }
}
```

✅ Following ISP
```TS
interface Worker {
  work(): void;
}

interface ManagerRole {
  manage(): void;
}

interface DeveloperRole {
  writeCode(): void;
}

class Developer implements Worker, DeveloperRole {
  work() {}
  writeCode() {}
}

class Manager implements Worker, ManagerRole {
  work() {}
  manage() {}
}
```

Key idea: Many small interfaces are better than one “fat” interface.
