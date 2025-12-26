# SOLID

SOLID is a set of five software design principles that help create code that is maintainable, extensible, testable, and scalable.

- **S — Single Responsibility Principle**
- **O — Open / Closed Principle**
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
