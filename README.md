# Cinematique - Movie Rental Dashboard

> **📦 ARCHIVED PROJECT** - This is a completed school project maintained for portfolio and educational reference purposes. No active development or updates are planned.

A comprehensive desktop application for managing movie rentals, built with **Java 16**, **JavaFX**, and **MySQL**. This full-featured system includes user authentication, movie browsing, rental management, and a complete admin dashboard for system administration.

## Project Overview

Cinematique is a school project that demonstrates proficiency in:
- **Desktop GUI Development** with JavaFX and FXML
- **Database Management** with MySQL and JDBC
- **MVC Architecture** and design patterns
- **User Authentication** and role-based access control
- **Modern UI/UX** with CSS styling

### Key Features

- **User System**
  - User registration and authentication
  - Profile management
  - Rental history tracking

- **Movie Rental**
  - Browse and search movies
  - Add movies to cart
  - Process rentals
  - View rental details and status

- **Admin Dashboard**
  - User management (create, view, delete)
  - Movie inventory management
  - Rental management and tracking
  - System logs and audit trail
  - Statistics and reporting

- **Social Features**
  - Movie reviews and ratings
  - User comments and feedback

## Technology Stack

| Component | Technology |
|-----------|-----------|
| **GUI Framework** | JavaFX 21 |
| **Language** | Java 16 |
| **Build Tool** | Gradle |
| **Database** | MySQL |
| **Database Driver** | MySQL Connector/J |
| **Markup** | FXML |
| **Styling** | CSS |
| **Module System** | Java Modules (JPMS) |

### Dependencies

- JavaFX 21 (controls, fxml, web, swing, media)
- ControlsFX 11.1.2
- FormsFX 11.6.0
- ValidatorFX 0.4.0
- Ikonli 12.3.1
- BootstrapFX 0.4.0
- TilesFX 11.48
- FXGL 17.3
- JUnit 5.10.0 (testing)

## Project Structure

```
src/main/java/com/schoolproject/
├── database/                          # Database layer
│   ├── DatabaseConnection.java        # JDBC connection management
│   ├── Functions.java                 # Generic database functions
│   ├── UserFunctions.java             # User-related queries
│   ├── MovieFunctions.java            # Movie-related queries
│   ├── LogFunctions.java              # Logging functionality
│   └── *.sql                          # Database initialization scripts
│
└── movie_rentaldashboard/             # GUI layer
    ├── *Controller.java               # FXML controllers
    ├── authentication/                # Authentication module
    ├── dao/                           # Data Access Objects
    ├── model/                         # Data models
    ├── global/                        # Global utilities
    └── resources/
        └── *.fxml                     # JavaFX UI markup
```

## Prerequisites

- **Java Development Kit (JDK)**: Java 16 or later
- **MySQL Server**: 5.7 or later
- **Gradle**: Version 7.0 or later (or use included wrapper)
- **Git**: For cloning and version control

## Installation & Setup

### 1. Clone the Repository

```bash
git clone https://github.com/HanazonoArchive/cinematique.git
cd cinematique
```

### 2. Database Setup

#### Option A: Using IntelliJ IDEA (Recommended)

1. Open the project in IntelliJ IDEA
2. Navigate to **View → Tool Windows → Database**
3. Click **+ → Data Source → MySQL**
4. Configure with your MySQL credentials
5. Open `src/main/java/com/schoolproject/database/initDb.sql`
6. Right-click and execute with the localhost data source

#### Option B: Manual MySQL Setup

```bash
# Connect to MySQL
mysql -u root -p

# Run initialization script
source src/main/java/com/schoolproject/database/initDb.sql;
source src/main/java/com/schoolproject/database/populateDB.sql;
```

### 3. Configure Database Connection

Edit `src/main/java/com/schoolproject/database/DatabaseConnection.java`:

```java
private static final String JDBC_URL = "jdbc:mysql://localhost:3306/cceprojectdatabase";
private static final String USERNAME = "your_username";
private static final String PASSWORD = "your_password";
```

### 4. Build the Project

```bash
# Using Gradle wrapper (Windows)
./gradlew build

# Using Gradle wrapper (Linux/Mac)
./gradlew build

# Or using your system Gradle
gradle build
```

### 5. Run the Application

```bash
# Using Gradle
./gradlew run

# Or run the main class
java -m com.schoolproject.movie_rentaldashboard/com.schoolproject.movie_rentaldashboard.HelloApplication
```

## Usage

### Default Credentials

**Admin Account:**
- Username: `admin`
- Password: `admin123`

**Sample User Account:**
- Username: `user`
- Password: `password123`

### Main Screens

1. **Login Screen** - User and admin authentication
2. **Home Screen** - Movie browsing and rental
3. **Admin Dashboard** - System administration
4. **User Profile** - Account management
5. **Cart** - Rental cart management
6. **Social Feed** - Reviews and ratings

## Database Schema

The application uses the following main tables:

- `users` - User account information
- `movies` - Movie inventory
- `rentals` - Rental transactions
- `reviews` - Movie reviews and ratings
- `logs` - System audit logs

See `src/main/java/com/schoolproject/database/initDb.sql` for complete schema.

## Build with Gradle

### Available Tasks

```bash
# Build the project
./gradlew build

# Run the application
./gradlew run

# Clean build artifacts
./gradlew clean

# Create distribution
./gradlew jlink

# Run tests
./gradlew test
```

## Development Notes

- The application uses **Java Modules (JPMS)** - see `src/main/java/module-info.java`
- **MVC Pattern** with controllers managing FXML views
- **DAO Pattern** for database abstraction
- **CSS Styling** for consistent UI theming

## Project Configuration

Key configuration files:
- `build.gradle` - Gradle build configuration
- `settings.gradle` - Gradle project settings
- `module-info.java` - Java module declarations

## Known Issues & Limitations

- Database credentials are currently hardcoded (consider using environment variables for production)
- Admin dashboard is view-only in some areas
- Limited error handling in database operations

## Future Enhancements

- [ ] Implement password hashing and security best practices
- [ ] Add email notifications for rentals
- [ ] Implement payment processing
- [ ] Add rental period management
- [ ] Create mobile companion app
- [ ] Implement real-time notifications

## Contributing

This is a school project and portfolio piece. While contributions are not actively solicited, suggestions and feedback are welcome. See [CONTRIBUTING.md](.github/CONTRIBUTING.md) for guidelines.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Author
**Jay Mark Agsoy**
**Eduard Anthony Pechayco**
**Mark Jade Palma**
**Paolo Andrew Pomar**

## Acknowledgments

- JavaFX community and documentation
- MySQL documentation
- Gradle ecosystem and plugins
- ControlsFX and related libraries
- ClassCode Educational materials

## Contact & Support

For questions or support regarding this project, please open an issue on GitHub.

---

**Last Updated:** May 2026  
**Java Version:** 16  
**JavaFX Version:** 21
