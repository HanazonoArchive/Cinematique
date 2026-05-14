# Frequently Asked Questions (FAQ)

> **📦 ARCHIVED PROJECT:** This is a completed school project maintained for portfolio reference. See the [README](README.md) for details.

## Getting Started

### Q: How do I get the project running?
**A:** Follow these steps:
1. Clone the repository: `git clone https://github.com/HanazonoArchive/cinematique.git`
2. Set up MySQL database (see [DATABASE_SETUP.md](docs/DATABASE_SETUP.md))
3. Build: `./gradlew build`
4. Run: `./gradlew run`

See [README.md](README.md) for detailed instructions.

### Q: Do I need any special IDE?
**A:** IntelliJ IDEA (Community or Ultimate) is recommended. You can also use:
- NetBeans
- Eclipse with JavaFX plugin
- VS Code with Java Extension Pack

### Q: What Java version do I need?
**A:** Java 16 or later. Check with `java -version`.

---

## Installation & Setup

### Q: I'm getting "MySQL Connection Refused" error
**A:** Ensure:
- MySQL is installed and running
- Port 3306 is not blocked
- Credentials are correct in `DatabaseConnection.java`
- Try: `mysql -u root -p` from command line

See [DATABASE_SETUP.md](docs/DATABASE_SETUP.md#troubleshooting) for more solutions.

### Q: How do I change the database username/password?
**A:** Edit `src/main/java/com/schoolproject/database/DatabaseConnection.java`:
```java
private static final String USERNAME = "your_username";
private static final String PASSWORD = "your_password";
```

Then restart the application.

### Q: Can I use a remote MySQL server?
**A:** Yes! Update the JDBC URL in `DatabaseConnection.java`:
```java
private static final String JDBC_URL = "jdbc:mysql://remote.server.com:3306/cceprojectdatabase";
```

### Q: Where are the database files?
**A:** They're managed by MySQL server. The SQL scripts are in:
```
src/main/java/com/schoolproject/database/
├── initDb.sql          # Create tables & structure
├── populateDB.sql      # Sample data
└── testPopulateDB.sql  # Test data
```

---

## Building & Running

### Q: What does `./gradlew build` do?
**A:** It:
- Compiles Java source code
- Resolves dependencies
- Packages the application
- Runs tests (if any)

### Q: How do I run just the application without building?
**A:** `./gradlew run` (it builds first if needed)

### Q: How do I clean the build?
**A:** `./gradlew clean` removes build artifacts

### Q: Can I build without running tests?
**A:** `./gradlew build -x test`

### Q: How do I create a distributable JAR?
**A:** The build process creates a JAR in `build/libs/`. Use `./gradlew jlink` for a complete distribution package.

---

## Development

### Q: How do I add a new feature?
**A:** 1. Create FXML file for UI
2. Create Controller class
3. Wire up database functions if needed
4. Test thoroughly

See [DEVELOPMENT.md](docs/DEVELOPMENT.md#adding-features) for detailed guide.

### Q: How do I debug the application?
**A:** 1. Set breakpoints in your IDE
2. Run → Debug (or Shift+F9 in IntelliJ)
3. Use Step Over/Into/Out buttons
4. Inspect variables in Debug panel

### Q: How do I add a new database table?
**A:** 1. Write SQL in a new file
2. Create functions class for queries
3. Execute SQL against the database
4. Update connection if needed

See [DEVELOPMENT.md](docs/DEVELOPMENT.md#adding-database-table).

### Q: What's the project structure?
**A:** Controllers handle UI, Functions handle database queries, Models represent data. See [DEVELOPMENT.md](docs/DEVELOPMENT.md#code-organization).

---

## Errors & Troubleshooting

### Q: I get "FXML file not found" error
**A:** 
- Check file is in `src/main/resources/com/schoolproject/movie_rentaldashboard/`
- Ensure controller path is correct in FXML
- Rebuild with `./gradlew clean build`

### Q: Application compiles but won't run
**A:** Common causes:
- MySQL not running: Start the service
- Database not initialized: Run SQL scripts
- Incorrect credentials: Update DatabaseConnection.java
- JavaFX path issue: Check module-info.java

### Q: I see module not found errors
**A:** Try:
- `./gradlew clean build`
- Invalidate IDE caches and restart
- Check `src/main/java/module-info.java` is present

### Q: How do I see detailed error messages?
**A:** Check:
- IDE console output (View → Tool Windows → Debug/Run)
- MySQL error logs
- Add `System.out.println()` for debugging

### Q: The UI looks broken/misaligned
**A:** - Check screen resolution (app expects 1152x850)
- Verify CSS files are loaded
- Check FXML layout constraints
- Rebuild and restart

---

## Database

### Q: How do I reset the database?
**A:** ```sql
DROP DATABASE cceprojectdatabase;
SOURCE src/main/java/com/schoolproject/database/initDb.sql;
SOURCE src/main/java/com/schoolproject/database/populateDB.sql;
```

### Q: Can I backup the database?
**A:** Yes! `mysqldump -u root -p cceprojectdatabase > backup.sql`

### Q: I accidentally deleted data, can I restore it?
**A:** Reset the database as shown above, or restore from backup if you have one.

### Q: What are the default test credentials?
**A:** After running populateDB.sql:
- Admin: username `admin`, password `admin123`
- User: username `john_doe`, password `password123`

### Q: Can I see what SQL queries are being executed?
**A:** Enable query logging in MySQL or add debug output in Functions.java

---

## Performance & Issues

### Q: The application runs slowly
**A:** 
- Ensure database is running locally or has good network connection
- Check database for missing indexes
- Look for N+1 query problems
- Use pagination for large datasets

### Q: I'm running out of memory
**A:** Increase JVM heap size:
```bash
./gradlew run -Dorg.gradle.jvmargs="-Xmx2g"
```

### Q: Multiple instances won't run
**A:** Only one instance can run at a time (single user application design)

---

## Contributing & Community

### Q: How do I contribute changes?
**A:** See [CONTRIBUTING.md](.github/CONTRIBUTING.md) for full guidelines.

### Q: Can I fork this project?
**A:** Yes! It's MIT Licensed. See [LICENSE](LICENSE).

### Q: Where do I report bugs?
**A:** Open an issue on GitHub with details. See [Bug Report Template](.github/ISSUE_TEMPLATE/bug_report.md).

### Q: Is this project actively maintained?
**A:** This is a school/portfolio project. Maintenance is limited but issues are welcomed.

---

## Advanced Topics

### Q: Can I package this as a Windows/Mac executable?
**A:** Yes, using `./gradlew jlink` creates a runtime image. You can then package with tools like Launch4j (Windows) or jpackage (Java 16+).

### Q: How do I deploy this to production?
**A:** ⚠️ **NOT RECOMMENDED** - This is educational software. For production:
- Implement proper security (password hashing, encryption, etc.)
- Use environment variables for credentials
- Add comprehensive error handling
- Implement proper logging
- Deploy on secure infrastructure
- See [SECURITY.md](.github/SECURITY.md) for details

### Q: Can I modify the UI styling?
**A:** Yes! Edit CSS files in `src/main/resources/com/schoolproject/movie_rentaldashboard/` or FXML layouts.

### Q: How do I add new dependencies?
**A:** Add to `build.gradle` in the dependencies section:
```gradle
implementation 'group:artifact:version'
```

Then run `./gradlew build` to download.

---

## Still Have Questions?

1. **Check Documentation:** [README.md](README.md), [DEVELOPMENT.md](docs/DEVELOPMENT.md)
2. **Search Issues:** Check if someone asked before
3. **Create Issue:** Open a new GitHub issue with details
4. **Check Code:** Review relevant source files for examples

---

**Last Updated:** May 2026
