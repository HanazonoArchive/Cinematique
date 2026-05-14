# Cinematique - Project Architecture

> **📦 ARCHIVED PROJECT:** This architecture documentation is provided for learning and reference purposes. The project is completed and not actively maintained.

This document provides an overview of the Cinematique project architecture, design patterns, and system organization.

## Table of Contents

1. [System Architecture](#system-architecture)
2. [Layered Architecture](#layered-architecture)
3. [Design Patterns](#design-patterns)
4. [Module Organization](#module-organization)
5. [Data Flow](#data-flow)
6. [Key Components](#key-components)

## System Architecture

```
┌─────────────────────────────────────────────────────┐
│          Cinematique Movie Rental System             │
└─────────────────────────────────────────────────────┘
                        │
        ┌───────────────┼───────────────┐
        │               │               │
   ┌────▼────┐  ┌──────▼──────┐  ┌──────▼──────┐
   │   GUI   │  │ Controllers │  │  User Input │
   │ (JavaFX)│  │  (MVC)      │  │  Handling   │
   └────┬────┘  └──────┬──────┘  └──────┬──────┘
        │              │                │
        └──────────────┼────────────────┘
                       │
        ┌──────────────▼──────────────┐
        │   Business Logic Layer      │
        │  (Functions, Services)      │
        └──────────────┬──────────────┘
                       │
        ┌──────────────▼──────────────┐
        │   Data Access Layer (DAO)   │
        │  (Database Functions)       │
        └──────────────┬──────────────┘
                       │
        ┌──────────────▼──────────────┐
        │   Data Persistence Layer    │
        │   (MySQL Database)          │
        └─────────────────────────────┘
```

## Layered Architecture

### 1. **Presentation Layer (GUI)**
**Location:** `com.schoolproject.movie_rentaldashboard`
- **Technology:** JavaFX + FXML
- **Responsibility:** Display UI, capture user input
- **Components:**
  - FXML Files (UI definitions)
  - CSS Files (styling)
  - Controllers (handle user interactions)

**Key Files:**
```
movie_rentaldashboard/
├── *Controller.java          # UI logic & event handling
├── resources/*.fxml          # UI layout definitions
└── resources/*.css           # Styling
```

**Example:**
```java
public class MovieCardController {
    @FXML private ImageView moviePoster;
    @FXML private Label movieTitle;
    
    @FXML
    private void handleRentClick() {
        // Handle rent button click
    }
}
```

### 2. **Business Logic Layer**
**Location:** `com.schoolproject.movie_rentaldashboard`
- **Responsibility:** Core business rules and operations
- **Components:**
  - Service classes
  - Validators
  - Data models

**Key Elements:**
- User authentication logic
- Rental calculations
- Payment processing
- Business rules validation

### 3. **Data Access Layer (DAO)**
**Location:** `com.schoolproject.database`
- **Responsibility:** Database operations
- **Technology:** JDBC, SQL
- **Components:**
  - Functions classes (UserFunctions, MovieFunctions, etc.)
  - SQL operations (CRUD)

**Key Files:**
```
database/
├── DatabaseConnection.java   # Connection pooling
├── UserFunctions.java        # User CRUD & queries
├── MovieFunctions.java       # Movie CRUD & queries
├── LogFunctions.java         # Logging operations
└── Functions.java            # Generic utilities
```

**Example:**
```java
public class MovieFunctions {
    public static List<Movie> getAllMovies() {
        try (Connection conn = DatabaseConnection.getConnection();
             Statement stmt = conn.createStatement()) {
            ResultSet rs = stmt.executeQuery("SELECT * FROM movies");
            // Process results
        }
    }
}
```

### 4. **Data Persistence Layer**
**Location:** MySQL Database
- **Responsibility:** Persistent data storage
- **Technology:** MySQL 5.7+
- **Tables:** users, movies, rentals, reviews, logs

## Design Patterns

### 1. **Model-View-Controller (MVC)**

```
MODEL (Data)              VIEW (UI)              CONTROLLER (Logic)
┌──────────────┐         ┌────────────┐         ┌────────────────┐
│  Movie.java  │◄────────│ FXML View  │────────►│ MovieController│
│  User.java   │         │            │         │                │
│  Rental.java │         └────────────┘         └────────────────┘
└──────────────┘              ▲                        │
                              │                        │
                              └────────────────────────┘
                           Update & Notify
```

**Implementation:**
- **Model:** POJO classes representing domain objects
- **View:** FXML files defining UI layout
- **Controller:** Java classes handling user interactions

### 2. **Data Access Object (DAO)**

```
┌──────────────────────────────────┐
│        Business Logic            │
└──────────┬───────────────────────┘
           │
┌──────────▼───────────────────────┐
│        DAO Pattern               │
│  - UserFunctions                 │
│  - MovieFunctions                │
│  - LogFunctions                  │
└──────────┬───────────────────────┘
           │
┌──────────▼───────────────────────┐
│        Database Layer            │
│  DatabaseConnection              │
│  JDBC Operations                 │
└──────────────────────────────────┘
```

**Benefits:**
- Abstracts database operations
- Allows easy switching of persistence layer
- Centralizes SQL logic

### 3. **Singleton Pattern**

**DatabaseConnection:**
```java
public class DatabaseConnection {
    private static final String JDBC_URL = "...";
    private static final String USERNAME = "...";
    
    // Static method providing single connection point
    public static Connection getConnection() throws SQLException {
        return DriverManager.getConnection(JDBC_URL, USERNAME, PASSWORD);
    }
}
```

### 4. **Factory Pattern**

```java
// Controllers created via FXMLLoader
FXMLLoader loader = new FXMLLoader(getClass().getResource("view.fxml"));
Parent root = loader.load();
// Controller instantiated automatically
```

## Module Organization

### Java Module System (JPMS)

**module-info.java:**
```java
module com.schoolproject.movie_rentaldashboard {
    requires javafx.controls;
    requires javafx.fxml;
    requires java.sql;
    // ... other requires
    
    opens com.schoolproject.movie_rentaldashboard to javafx.fxml;
    exports com.schoolproject.movie_rentaldashboard;
}
```

**Benefits:**
- Strong encapsulation
- Clear dependencies
- Version compatibility

## Data Flow

### 1. **User Login Flow**

```
┌─────────────────────────────────────────────┐
│ 1. User enters credentials in LoginView     │
└────────────────┬────────────────────────────┘
                 │
┌────────────────▼─────────────────────────┐
│ 2. LoginController.handleLogin()         │
│    - Get credentials from UI             │
└────────────────┬────────────────────────┘
                 │
┌────────────────▼─────────────────────────────┐
│ 3. BusinessLogic.authenticateUser()         │
│    - Validate credentials                   │
└────────────────┬────────────────────────────┘
                 │
┌────────────────▼─────────────────────────────┐
│ 4. UserFunctions.getUserByUsername()        │
│    - Query database                         │
└────────────────┬────────────────────────────┘
                 │
┌────────────────▼─────────────────────────────┐
│ 5. Database returns User object             │
└────────────────┬────────────────────────────┘
                 │
┌────────────────▼─────────────────────────────┐
│ 6. Validate password match                  │
└────────────────┬────────────────────────────┘
                 │
        ┌────────┴────────┐
        │                 │
   ┌────▼────┐       ┌────▼────┐
   │  Valid  │       │ Invalid │
   │ Success │       │ Failure │
   └────┬────┘       └────┬────┘
        │                 │
    ┌───▼──────┐      ┌───▼────────┐
    │Load Home │      │Show Error  │
    │Screen   │       │Message     │
    └──────────┘      └────────────┘
```

### 2. **Movie Rental Flow**

```
┌────────────────────────────────────────┐
│ 1. User selects movie & clicks Rent    │
└────────────────┬───────────────────────┘
                 │
┌────────────────▼───────────────────────┐
│ 2. MovieCardController.handleRent()    │
└────────────────┬───────────────────────┘
                 │
┌────────────────▼──────────────────────────┐
│ 3. Add movie to user's rental cart       │
└────────────────┬──────────────────────────┘
                 │
┌────────────────▼──────────────────────────┐
│ 4. RentalFunctions.createRental()        │
│    - Insert into rentals table           │
└────────────────┬──────────────────────────┘
                 │
┌────────────────▼──────────────────────────┐
│ 5. Update movie availability             │
│    - Decrease quantity in stock          │
└────────────────┬──────────────────────────┘
                 │
┌────────────────▼──────────────────────────┐
│ 6. Log rental action                     │
│    - Insert into logs table              │
└────────────────┬──────────────────────────┘
                 │
┌────────────────▼──────────────────────────┐
│ 7. Update UI with confirmation           │
└──────────────────────────────────────────┘
```

## Key Components

### Controllers (View Controllers)

Handles user interactions and updates the view:

```
Controllers/
├── LoginController           # Authentication screen
├── HomeController            # Main home screen
├── MovieCardController       # Individual movie display
├── CartController            # Shopping cart
├── AdminMainScreenController # Admin dashboard
└── ...
```

### Functions (Data Access)

Database query operations:

```
Functions/
├── UserFunctions             # User CRUD
├── MovieFunctions            # Movie CRUD
├── RentalFunctions           # Rental CRUD
├── ReviewFunctions           # Review operations
└── LogFunctions              # Audit logging
```

### Models (Domain Objects)

Data models:

```
Models/
├── User.java                 # User entity
├── Movie.java                # Movie entity
├── Rental.java               # Rental entity
├── Review.java               # Review entity
└── Cart.java                 # Shopping cart
```

### Database

```
MySQL/
├── users                     # User accounts
├── movies                    # Movie inventory
├── rentals                   # Rental records
├── reviews                   # Movie reviews
└── logs                      # Audit trail
```

## Sequence Diagram Example

### Login Sequence

```
User        LoginView      Controller        DB              Cache
 │              │              │             │               │
 ├─Enter ID────►│              │             │               │
 │              │              │             │               │
 ├─Click Login─►│              │             │               │
 │              │              │             │               │
 │              ├─Validate────►│             │               │
 │              │              │             │               │
 │              │              ├─Query ID──►│               │
 │              │              │             │               │
 │              │              │◄─Result────┤               │
 │              │              │             │               │
 │              │◄─Result──────┤             │               │
 │              │              │             │               │
 │◄─Dashboard───┤              │             │               │
 │              │              │             │      Cache ID │
 │              │              │             │◄──────────────┤
```

## Dependencies & Libraries

### Core JavaFX
- `javafx.controls` - UI controls
- `javafx.fxml` - FXML XML processor
- `javafx.web` - Web view
- `javafx.media` - Media playback

### Database
- `mysql-connector-java` - MySQL JDBC driver

### Additional Libraries
- ControlsFX - Extended JavaFX controls
- FormsFX - Form building
- ValidatorFX - Input validation
- FXGL - Game library framework

## Performance Considerations

1. **Connection Management**
   - Connections created per query
   - Connection pooling could improve performance

2. **Data Caching**
   - No current caching mechanism
   - Consider caching frequently accessed movies

3. **UI Updates**
   - Run database queries off JavaFX thread
   - Use Platform.runLater() for UI updates

4. **Pagination**
   - Load movies in batches
   - Implement lazy loading

## Security Notes

⚠️ **Educational Project** - Not production-ready

Current issues:
- Hardcoded credentials
- Unencrypted passwords (possibly)
- No input validation in some areas
- SQL injection vulnerability potential
- No SSL/TLS for database connection

## Testing Strategy

Currently lacks comprehensive tests. For future:
- Unit tests for Functions classes
- Integration tests for workflows
- UI testing with TestFX
- Database tests with H2 in-memory DB

## Deployment Architecture

```
┌──────────────┐
│ Application  │  Desktop JAR/Executable
│  Package     │
└──────┬───────┘
       │
┌──────▼───────────────────────┐
│    User's Machine             │
│  ┌────────────────────────┐  │
│  │ JavaFX Runtime (JRE)   │  │
│  └────────────────────────┘  │
│  ┌────────────────────────┐  │
│  │ Local MySQL Connection │  │
│  └────────────────────────┘  │
└──────────────────────────────┘
```

---

**For detailed setup, see:**
- [README.md](../README.md)
- [DEVELOPMENT.md](DEVELOPMENT.md)
- [DATABASE_SETUP.md](DATABASE_SETUP.md)
