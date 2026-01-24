# Library Management System

## Project Description

A comprehensive Library Management System built with ASP.NET Core and Entity Framework. The application allows users to manage books and borrowing records with full CRUD operations, advanced search filtering, pagination, and automatic status management.

### Key Features:
- Create, Read, Update, Delete (CRUD) operations for borrowing records
- Advanced pagination with 10 records per page
- Search functionality by book title, ISBN, or borrower name
- Filter borrowings by status (All, Borrowed, Returned, Overdue)
- Automatic availability management for books
- Return book functionality with status tracking
- Comprehensive input validation
- Overdue detection and status calculation

## Technology Stack
- **.NET Framework**: .NET 10.0
- **Web Framework**: ASP.NET Core (Razor Pages)
- **ORM**: Entity Framework Core 10.0.0
- **Database**: SQL Server (LocalDB)
- **Frontend**: HTML5, Bootstrap 5, CSS
- **Language**: C#, Razor

## Database Setup

### Database Schema

#### Books Table
- `id` (int, PK, Auto-increment)
- `isbn` (string, NOT NULL, UNIQUE)
- `title` (string, NOT NULL)
- `author` (string, NOT NULL)
- `publisher` (string, nullable)
- `publication_year` (int, nullable)
- `category` (string, NOT NULL)
- `total_copies` (int, NOT NULL)
- `available_copies` (int, NOT NULL)
- `created_at` (datetime)
- `updated_at` (datetime)

#### Borrowings Table
- `id` (int, PK, Auto-increment)
- `book_id` (int, FK, NOT NULL)
- `borrower_name` (string, NOT NULL, 2-100 chars)
- `borrower_phone` (string, NOT NULL, 10-11 digits)
- `borrower_email` (string, NOT NULL, unique)
- `borrow_date` (datetime, NOT NULL)
- `due_date` (datetime, NOT NULL)
- `return_date` (datetime, nullable)
- `status` (string, NOT NULL) - Borrowed/Returned/Overdue
- `created_at` (datetime)
- `updated_at` (datetime)

### Seeded Data
- **12 Books** with realistic library data
- **31 Borrowing Records** with various statuses (Borrowed, Returned, Overdue)

### Database Initialization

The application uses Entity Framework migrations to automatically create and seed the database.

```bash
# Update database (create tables and seed data)
dotnet ef database update
```

The migration will be automatically applied when the application starts.

## Features Implemented

### Completed Features

- [x] Database setup with relationships (Question 1)
  - [x] Created LibraryDbContext with proper configuration
  - [x] Implemented models with Data Annotations
  - [x] Created migrations for database schema
  - [x] Seeded 12 books and 31 borrowing records

- [x] Create Borrowing (Question 2.1)
  - [x] Form validation for all fields
  - [x] Phone number validation (10-11 digits)
  - [x] Email validation and uniqueness
  - [x] Book availability check
  - [x] Automatic status setting to "Borrowed"
  - [x] Automatic available_copies decrement

- [x] Read Borrowing (Question 2.2)
  - [x] List all borrowings with pagination (10 per page)
  - [x] Search by book title, ISBN, or borrower name
  - [x] Filter by status
  - [x] Display all required information
  - [x] Overdue status calculation
  - [x] Pagination controls

- [x] Update Borrowing (Question 2.3)
  - [x] Edit only when status is "Borrowed"
  - [x] Validate all input fields
  - [x] Prevent editing of Book ID and Borrow Date
  - [x] Due date validation
  - [x] Email uniqueness check

- [x] Delete Borrowing (Question 2.4)
  - [x] Delete only when status is "Returned"
  - [x] Confirmation dialog
  - [x] Automatic available_copies management

- [x] Return Book (Bonus Question 3)
  - [x] Dedicated return book page
  - [x] Search by ISBN or Book Title
  - [x] Automatic return_date setting
  - [x] Status management (Returned/Overdue)
  - [x] Automatic available_copies increment
  - [x] Overdue detection on return

## How to Run

### Prerequisites
- .NET 10.0 SDK
- SQL Server (LocalDB or Express)
- Visual Studio Code or Visual Studio (optional)

### Installation and Running

1. **Clone/Navigate to the project**
```bash
cd d:\backend\FIT4016-KiemTraLan2-2026\LibraryManagementApp
```

2. **Restore NuGet packages**
```bash
dotnet restore
```

3. **Create and seed database**
```bash
dotnet ef database update
```

4. **Build the project**
```bash
dotnet build
```

5. **Run the application**
```bash
dotnet run
```

The application will be available at:
- **HTTP**: `http://localhost:5000`
- **HTTPS**: `https://localhost:5001`

### Using Visual Studio
1. Open the solution in Visual Studio
2. Set `LibraryManagementApp` as the startup project
3. Press F5 or click "Run"

## Application Usage

### Home Page
- Displays welcome message and feature overview
- Quick navigation to borrowings management

### Borrowings Page (`/Borrowings`)

#### Create New Borrowing
1. Click "Create New Borrowing" button
2. Select a book from the dropdown
3. Enter borrower information:
   - Name (2-100 characters)
   - Phone (10-11 digits)
   - Email (valid email format)
4. Set borrow date and due date
5. Submit form
6. System automatically:
   - Decreases available_copies
   - Sets status to "Borrowed"
   - Records creation timestamp

#### View Borrowings
- Displays paginated list (10 per page)
- Shows book info, borrower info, dates, and status
- Pagination controls at bottom
- Record count and page information

#### Search & Filter
- **Search**: Enter text to search by book title, ISBN, or borrower name
- **Filter**: Select status (All, Borrowed, Returned, Overdue)
- Results update dynamically

#### Edit Borrowing
1. Click "Edit" button (only available for "Borrowed" status)
2. Update: Borrower Name, Phone, Email, Due Date
3. Cannot modify: Book ID, Borrow Date
4. Submit changes

#### Return Book
1. Navigate to Return Book page
2. Choose search method (ISBN or Title)
3. Enter book information
4. System finds the latest unreturn borrowing
5. Click "Confirm Return Book"
6. System automatically:
   - Sets return_date to current date
   - Updates status to "Returned" or "Overdue"
   - Increments available_copies

#### Delete Borrowing
1. Click "Delete" button (only available for "Returned" status)
2. Review borrowing details
3. Confirm deletion
4. Record is removed from database
5. Available copies updated if no more "Borrowed" records

## Project Structure

```
LibraryManagementApp/
├── Models/
│   ├── Book.cs                    # Book entity model
│   └── Borrowing.cs               # Borrowing entity model
├── Data/
│   ├── LibraryDbContext.cs        # Database context
│   └── Migrations/                # EF migrations
├── Services/
│   ├── IBookService.cs            # Book service interface
│   ├── BookService.cs             # Book service implementation
│   ├── IBorrowingService.cs       # Borrowing service interface
│   └── BorrowingService.cs        # Borrowing service implementation
├── Pages/
│   ├── Index.cshtml               # Home page
│   ├── Index.cshtml.cs            # Home page model
│   ├── Borrowings/
│   │   ├── Index.cshtml           # List borrowings
│   │   ├── Index.cshtml.cs        # List borrowings model
│   │   ├── Create.cshtml          # Create borrowing
│   │   ├── Create.cshtml.cs       # Create borrowing model
│   │   ├── Edit.cshtml            # Edit borrowing
│   │   ├── Edit.cshtml.cs         # Edit borrowing model
│   │   ├── Delete.cshtml          # Delete borrowing
│   │   ├── Delete.cshtml.cs       # Delete borrowing model
│   │   ├── Details.cshtml         # View borrowing details
│   │   ├── Details.cshtml.cs      # View borrowing details model
│   │   ├── Return.cshtml          # Return book page
│   │   └── Return.cshtml.cs       # Return book model
│   ├── Shared/
│   │   ├── _Layout.cshtml         # Main layout
│   │   └── _ValidationScriptsPartial.cshtml
│   ├── _ViewImports.cshtml        # View imports
│   └── _ViewStart.cshtml          # View start
├── appsettings.json               # Configuration
├── Program.cs                     # Application startup
├── LibraryManagementApp.csproj    # Project file
└── README.md                      # This file
```

## Validation Rules

### Borrowing Creation/Update Validation

| Field | Rules |
|-------|-------|
| Borrower Name | Required, 2-100 characters |
| Borrower Phone | Required, 10-11 digits only |
| Borrower Email | Required, valid email format, unique |
| Book | Required, must exist in database |
| Book Availability | Required, available_copies > 0 |
| Borrow Date | Required, cannot be in future |
| Due Date | Required, >= Borrow Date, <= Borrow Date + 30 days |
| Status | Auto-set to "Borrowed" on creation |

### Status Rules

- **Borrowed**: Initial status when borrowing is created
- **Returned**: When book is returned on or before due date
- **Overdue**: When book is returned after due date, or when viewing list if return_date is null and due_date < today

## Error Handling

The application provides user-friendly error messages in English:
- Validation error messages
- Database error messages
- Not found messages
- Access denied messages (e.g., cannot edit "Returned" borrowings)

## Code Quality

- **Naming Conventions**: PascalCase for classes/methods, camelCase for variables
- **Code Organization**: Separation of concerns with Models, Data, Services, Pages
- **Comments**: Added for complex business logic
- **Validation**: Server-side and client-side validation
- **Error Handling**: Try-catch blocks in service methods

## Git Commit History

This project includes meaningful commits documenting the development process:

1. `Add Book and Borrowing models`
2. `Implement DbContext with relationships and seed data`
3. `Create Book and Borrowing services with business logic`
4. `Implement Create Borrowing with validation`
5. `Add Read with pagination and search filter`
6. `Implement Update with business logic`
7. `Add Delete with status check`
8. `Implement Return Book functionality (bonus)`
9. `Add Pages and UI for all CRUD operations`
10. `Fix validation and update layout`

## Performance Considerations

- Uses async/await for all database operations
- Lazy loading relationships where appropriate
- Pagination to limit data retrieval
- Efficient queries with LINQ

## Security Considerations

- SQL injection prevention through Entity Framework parameterized queries
- Email validation and uniqueness checks
- Phone number format validation
- Input length validation
- Status-based access control (cannot edit "Returned" borrowings)

## Future Enhancements

- Email notifications for overdue borrowings
- Book image uploads
- Advanced reporting and statistics
- User authentication and roles
- SMS notifications
- Fine calculations for overdue books
- Book category management
- Borrower management
- Transaction history

## Troubleshooting

### Database Connection Issues
1. Verify LocalDB is running: `sqllocaldb info`
2. Check connection string in `appsettings.json`
3. Clear migrations and re-create: `dotnet ef database drop` then `dotnet ef database update`

### Build Errors
1. Run `dotnet restore` to restore packages
2. Verify .NET 10.0 SDK is installed: `dotnet --version`
3. Clean build: `dotnet clean` then `dotnet build`

### Runtime Issues
1. Check that database migrations were applied
2. Verify ports 5000/5001 are not in use
3. Check application logs in console

## Author
[Student Name - Student ID]

## License
Educational Project - FIT4016 Assessment

## Contact
For questions or issues, please refer to the project requirements document.
