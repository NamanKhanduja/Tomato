# Tomato - Online Food Ordering System

An enterprise-grade C++ implementation of a food delivery application that demonstrates advanced design patterns and software architecture principles.

## 📋 Overview

Tomato is a comprehensive Online Food Ordering System built with C++ that simulates the core functionality of food delivery platforms like Swiggy or Zomato. The system implements industry-standard design patterns to create a scalable, maintainable, and extensible architecture.

## ✨ Key Features

- **Restaurant Management**: Browse and search restaurants by location
- **Menu Management**: View restaurant menus with items and pricing
- **Shopping Cart**: Add/remove items with real-time cost calculation
- **Order Processing**: Support for immediate and scheduled orders
- **Multiple Payment Methods**: UPI, Credit Card, and extensible payment strategies
- **Order Management**: Track and manage customer orders
- **Notification System**: Order confirmation and status notifications
- **User Profiles**: Manage user information and delivery locations

## 🏗️ Architecture & Design Patterns

### Design Patterns Implemented

1. **Facade Pattern** (`TomatoApp.h`)
   - Provides a simplified interface to the complex subsystem
   - Orchestrates interactions between restaurants, carts, and orders

2. **Singleton Pattern** (`RestaurantManager`, `OrderManager`)
   - Ensures single instance of managers throughout the application
   - Prevents duplicate data and maintains consistency

3. **Strategy Pattern** (`PaymentStrategy`)
   - Encapsulates payment algorithms
   - Supports multiple payment methods (UPI, Credit Card, etc.)
   - Easy to add new payment methods

4. **Factory Pattern** (`OrderFactory`, `NowOrderFactory`, `ScheduledOrderFactory`)
   - Creates different types of orders based on requirements
   - Decouples order creation from order usage
   - Handles immediate and scheduled order creation

5. **Service Layer Pattern** (`NotificationService`)
   - Handles cross-cutting concerns like notifications
   - Separates business logic from infrastructure concerns

## 📁 Project Structure

```
Tomato/
├── main.cpp                          # Entry point and application demo
├── TomatoApp.h                       # Facade class (main orchestrator)
│
├── models/                           # Data models
│   ├── MenuItem.h                   # Individual menu item
│   ├── Restaurant.h                 # Restaurant entity
│   ├── User.h                       # User/Customer entity
│   ├── Cart.h                       # Shopping cart
│   ├── Order.h                      # Abstract order class
│   ├── DeliveryOrder.h              # Delivery order type
│   └── PickupOrder.h                # Pickup order type
│
├── managers/                         # Singleton managers
│   ├── RestaurantManager.h          # Restaurant registry and search
│   └── OrderManager.h               # Order tracking and history
│
├── strategies/                       # Strategy pattern implementations
│   ├── PaymentStrategy.h            # Payment interface
│   ├── CreditCardPaymentStrategy.h  # Credit card payments
│   └── UpiPaymentStrategy.h         # UPI/Digital wallet payments
│
├── factories/                        # Factory pattern implementations
│   ├── OrderFactory.h               # Abstract factory
│   ├── NowOrderFactory.h            # Immediate order factory
│   └── ScheduledOrderFactory.h      # Scheduled order factory
│
├── services/                         # Business services
│   └── NotificationService.h        # Order notifications
│
└── utils/                            # Utility functions
    └── TimeUtils.h                  # Time and scheduling utilities
```

## 🚀 Getting Started

### Prerequisites

- C++ 11 or higher
- A C++ compiler (g++, clang, or MSVC)
- Standard C++ libraries

### Compilation

```bash
g++ -std=c++11 -o tomato main.cpp
```

### Running the Application

```bash
./tomato
```

### Sample Output

```
User: Aditya is active.
Found Restaurants:
 - Bikaner
Selected restaurant: Bikaner
Items in cart:
------------------------------------
P1 : Chole Bhature : ₹120
P2 : Samosa : ₹15
------------------------------------
Grand total : ₹135
[Order Confirmation] Order #12345 confirmed. Estimated delivery: 30 minutes
```

## 💡 Usage Examples

### Basic Order Flow

```cpp
// 1. Initialize the app
TomatoApp* tomato = new TomatoApp();

// 2. Create a user
User* user = new User(101, "Aditya", "Delhi");

// 3. Search restaurants by location
vector<Restaurant*> restaurants = tomato->searchRestaurants("Delhi");

// 4. Select a restaurant
tomato->selectRestaurant(user, restaurants[0]);

// 5. Add items to cart
tomato->addToCart(user, "P1");
tomato->addToCart(user, "P2");

// 6. View cart
tomato->printUserCart(user);

// 7. Checkout and pay
PaymentStrategy* payment = new UpiPaymentStrategy("1234567890");
Order* order = tomato->checkoutNow(user, "Delivery", payment);
tomato->payForOrder(user, order);
```

### Immediate Order
```cpp
Order* order = tomato->checkoutNow(user, "Delivery", new UpiPaymentStrategy("9876543210"));
```

### Scheduled Order
```cpp
Order* order = tomato->checkoutScheduled(user, "Delivery", 
                                         new CreditCardPaymentStrategy("4111111111111111"), 
                                         "2026-04-07 19:30");
```

## 🔧 Extending the System

### Adding a New Payment Method

1. Create a new class extending `PaymentStrategy`:
```cpp
class GooglePayPaymentStrategy : public PaymentStrategy {
public:
    GooglePayPaymentStrategy(const string& phoneNumber) : mobileNumber(phoneNumber) {}
    
    bool processPayment() override {
        // Implement Google Pay logic
        return true;
    }
private:
    string mobileNumber;
};
```

2. Use it in checkout:
```cpp
Order* order = tomato->checkoutNow(user, "Delivery", 
                                    new GooglePayPaymentStrategy("9999999999"));
```

### Adding a New Restaurant

```cpp
Restaurant* newRestaurant = new Restaurant("FatJoe's Burgers", "Mumbai");
newRestaurant->addMenuItem(MenuItem("B1", "Classic Burger", 250));
newRestaurant->addMenuItem(MenuItem("B2", "Cheese Burger", 300));

RestaurantManager::getInstance()->addRestaurant(newRestaurant);
```

## 📊 Core Components

### TomatoApp (Facade)
Central orchestrator that provides a simple interface to users while managing complex interactions between:
- Restaurant searches
- Cart management
- Order processing
- Payment handling
- Notifications

### Models
- **MenuItem**: Product with code, name, and price
- **Restaurant**: Collection of menu items with location
- **User**: Customer with ID, name, location, and cart
- **Cart**: Items container with total cost calculation
- **Order**: Abstract base with DeliveryOrder and PickupOrder implementations

### Managers (Singleton Pattern)
- **RestaurantManager**: Registry and search functionality for restaurants
- **OrderManager**: Tracks all orders and maintains order history

### Strategies (Strategy Pattern)
Pluggable payment processors that can be easily added without modifying existing code.

### Factories (Factory Pattern)
- **NowOrderFactory**: Creates orders for immediate fulfillment
- **ScheduledOrderFactory**: Creates orders for future delivery at specified times

## 🎯 Design Principles Applied

1. **SOLID Principles**
   - Single Responsibility: Each class has one reason to change
   - Open/Closed: Open for extension, closed for modification
   - Liskov Substitution: PaymentStrategy subclasses are interchangeable
   - Interface Segregation: Small, focused interfaces
   - Dependency Inversion: Depend on abstractions, not concretions

2. **DRY (Don't Repeat Yourself)**: Shared logic centralized in managers and services

3. **Separation of Concerns**: Business logic, data models, and infrastructure are separated

4. **Scalability**: Easy to add new payment methods, restaurants, and order types

## 📝 Example Workflow

1. User launches the app → TomatoApp initializes with sample restaurants
2. User searches for restaurants in their location
3. User selects a restaurant and adds items to cart
4. User reviews their order in the cart
5. User chooses payment method and completes checkout
6. Order is processed and registered with OrderManager
7. Payment is processed using selected strategy
8. Order confirmation notification is sent
9. Cart is cleared for next order

## 🔍 Key Classes Overview

| Class | Purpose |
|-------|---------|
| `TomatoApp` | Main facade and application controller |
| `Restaurant` | Represents a restaurant with menu items |
| `User` | Represents a customer with cart |
| `Cart` | Manages user's selected items |
| `Order` | Abstract base for different order types |
| `RestaurantManager` | Singleton managing all restaurants |
| `OrderManager` | Singleton managing all orders |
| `PaymentStrategy` | Abstract payment processor |
| `OrderFactory` | Abstract factory for order creation |
| `NotificationService` | Handles order notifications |

## 🛠️ Technologies & Tools

- **Language**: C++11
- **Paradigm**: Object-Oriented Programming (OOP)
- **Architecture**: Design Patterns-based Architecture
- **Build**: Standard C++ compilation

## 📚 Learning Outcomes

This project demonstrates:
- Practical implementation of industry-standard design patterns
- Clean code principles and architecture best practices
- Object-oriented design and polymorphism
- Singleton and Factory pattern usage
- Strategy pattern for algorithm encapsulation
- Facade pattern for simplifying complex systems
- Real-world system design and scalability

## 🤝 Contributing

Contributions are welcome! Feel free to:
- Report bugs or suggest features via GitHub Issues
- Submit pull requests for improvements
- Enhance documentation and examples
- Add new payment methods or order types

## 📄 License

This project is open source and available under the MIT License.

## 👨‍💻 Author

**Naman Khanduja**
- GitHub: [@NamanKhanduja](https://github.com/NamanKhanduja)
- Repository: [Tomato](https://github.com/NamanKhanduja/Tomato)

## 📞 Support

For questions or issues:
1. Check existing GitHub Issues
2. Create a new issue with detailed description
3. Include code snippets or error traces when applicable

---

**Made with ❤️ for learning and demonstrating software design patterns**