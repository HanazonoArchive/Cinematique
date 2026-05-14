# Cinematique Development Guide

> **📦 ARCHIVED PROJECT:** This project is completed and archived. Use this guide for learning and understanding the architecture. Code is not actively modified.

This document provides detailed guidance for developers studying and learning from the Cinematique project.

## Table of Contents

1. [Project Structure](#project-structure)
2. [Building & Running](#building--running)
3. [Database Setup](#database-setup)
4. [Code Organization](#code-organization)
5. [Adding Features](#adding-features)
6. [Debugging](#debugging)
7. [Common Tasks](#common-tasks)

## Project Structure

```
cinematique/
├── src/main/java/com/schoolproject/
│   ├── database/              # Database layer (JDBC, SQL)
│   │   ├── DatabaseConnection.java
│   │   ├── Functions.java
│   │   ├── UserFunctions.java
│   │   ├── MovieFunctions.java
│   │   ├── LogFunctions.java
│   │   └── *.sql
│   └── movie_rentaldashboard/
│       ├── *Controller.java   # FXML Controllers
│       ├── authentication/    # Auth module
│       ├── dao/              # Data Access Objects
│       ├── model/            # Data models
│       └── resources/        # FXML & CSS
├── src/main/resources/
│   └── com/schoolproject/movie_rentaldashboard/
│       ├── *.fxml            # JavaFX UI definitions
│       └── *.css             # Styling
├── build.gradle              # Build configuration
├── settings.gradle           # Project settings
├── README.md                 # User guide
├── LICENSE                   # MIT License
└── .github/
    ├── CONTRIBUTING.md       # Contribution guidelines
    └── pull_request_template.md

```

## Building & Running

### Quick Start

```bash
# Clone repository
git clone https://github.com/HanazonoArchive/cinematique.git
cd cinematique

# Build
./gradlew build

# Run
./gradlew run
```

### Gradle Tasks

```bash
./gradlew tasks                    # List all tasks
./gradlew clean                    # Clean build
./gradlew build                    # Build project
./gradlew run                      # Run application
./gradlew test                     # Run tests
./gradlew compileJava              # Compile only
./gradlew javadoc                  # Generate docs
./gradlew jlink                    # Create runtime image
```

## Database Setup

### MySQL Installation

1. **Install MySQL Server** (5.7 or later)
2. **Start MySQL service**
3. **Create root user password** (if not set)

### Initialize Database

#### Using IntelliJ IDEA:

1. View → Tool Windows → Database
2. Click **+ → Data Source → MySQL**
3. Enter credentials:
   - Host: localhost
   - Port: 3306
   - User: root (or your username)
4. Click **Test Connection**
5. Open `src/main/java/com/schoolproject/database/initDb.sql`
6. Right-click → Run (with your data source)
7. Execute `populateDB.sql` for sample data

#### Using Command Line:

```bash
mysql -u root -p < src/main/java/com/schoolproject/database/initDb.sql
mysql -u root -p < src/main/java/com/schoolproject/database/populateDB.sql
```

### Configure Connection

Edit `DatabaseConnection.java`:

```java
private static final String JDBC_URL = "jdbc:mysql://localhost:3306/cceprojectdatabase";
private static final String USERNAME = "your_username";  // Change this
private static final String PASSWORD = "your_password";  // Change this
```

## Code Organization

### Controller Pattern

Controllers handle user interactions and update the UI:

```
*Controller.java
├── FXML Injection (@FXML)
├── Event Handlers (onClick, etc.)
├── Data Binding
└── Database Calls
```

Example structure:
```java
public class MovieController {
    @FXML private ListView<Movie> movieList;
    @FXML private TextField searchField;
    
    @FXML
    public void initialize() {
        // Initialize UI components
    }
    
    @FXML
    private void handleSearch() {
        // Search logic
    }
}
```

### Database Functions

Database functions provide abstraction layer:

```java
public class MovieFunctions {
    public static List<Movie> getAllMovies() { }
    public static Movie getMovieById(int id) { }
    public static boolean addMovie(Movie movie) { }
    public static boolean updateMovie(Movie movie) { }
    public static boolean deleteMovie(int id) { }
}
```

### Model Classes

Models represent data entities:

```java
public class Movie {
    private int id;
    private String title;
    private String director;
    private int year;
    // Getters and setters
}
```

## Adding Features

### Adding a New Screen

1. **Create FXML file** in `resources/com/schoolproject/movie_rentaldashboard/`:
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <?import javafx.geometry.*?>
   <?import javafx.scene.control.*?>
   <?import javafx.scene.layout.*?>
   
   <VBox xmlns="http://javafx.com/javafx/21" xmlns:fx="http://javafx.com/fxml/1"
         fx:controller="com.schoolproject.movie_rentaldashboard.MyScreenController">
       <!-- UI Elements -->
   </VBox>
   ```

2. **Create Controller** in `movie_rentaldashboard/`:
   ```java
   public class MyScreenController {
       @FXML
       public void initialize() {
           // Initialize
       }
   }
   ```

3. **Register in Navigation** (usually in navigation controller)

### Adding Database Table

1. **Create SQL migration** in `database/`:
   ```sql
   CREATE TABLE new_table (
       id INT PRIMARY KEY AUTO_INCREMENT,
       name VARCHAR(255),
       created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
   );
   ```

2. **Create Functions class**:
   ```java
   public class NewTableFunctions {
       public static List<Item> getAll() { }
       public static Item getById(int id) { }
       public static boolean add(Item item) { }
   }
   ```

3. **Update `DatabaseConnection.java` if needed**

### Adding Styling

1. Modify or create CSS files in `resources/com/schoolproject/movie_rentaldashboard/`
2. Link in FXML: `<stylesheets><URL value="path/to/style.css"/></stylesheets>`
3. Use CSS classes: `.custom-button { /* styles */ }`

## Debugging

### Enable Debug Logging

Add debugging output:
```java
System.out.println("Debug: " + variable);
```

### Using IDE Debugger

1. **Set breakpoint** on a line
2. **Run in debug mode** (Shift+F9 in IntelliJ)
3. **Step through code** (F10=Step, F8=Step Over, F7=Step Into)
4. **Inspect variables** in Debug panel

### Common Issues

| Issue | Solution |
|-------|----------|
| Connection refused | MySQL not running; check credentials |
| FXML not found | Check resource path; rebuild project |
| Module not found | Check `module-info.java` exports |
| Compilation error | Run `./gradlew clean build` |
| UI not displaying | Check FXML layout; verify controller |

## Common Tasks

### Running Specific Class

```bash
java -cp ".:lib/*" com.schoolproject.database.DatabaseConnection
```

### Generating Documentation

```bash
./gradlew javadoc
# Output in: build/docs/javadoc/
```

### Packaging for Distribution

```bash
./gradlew jlink
# Output in: build/image/
```

### Cleaning Everything

```bash
./gradlew clean
rm -rf .gradle build/ build/*
```

## Performance Tips

- Close database connections after use
- Use List instead of loading all data at once (pagination)
- Cache frequently accessed data
- Minimize UI thread blocking with Threading

## Best Practices

- Follow single responsibility principle
- Keep methods focused and testable
- Use meaningful names for variables/methods
- Add comments for complex logic
- Test database operations
- Use try-catch for error handling
- Close resources (Connections, Statements, ResultSets)

## Git Workflow

```bash
# Create feature branch
git checkout -b feature/new-feature

# Make changes
# Commit
git commit -m "Add feature description"

# Push
git push origin feature/new-feature

# Create Pull Request on GitHub
```

## Resources

- [JavaFX Documentation](https://openjfx.io/)
- [MySQL JDBC Documentation](https://dev.mysql.com/doc/connector-j/en/)
- [Gradle Documentation](https://gradle.org/guides/)
- [Java Module System](https://www.oracle.com/corporate/features/understanding-java-9-modules/)

---

**For questions or issues, refer to README.md or open an issue on GitHub.**
