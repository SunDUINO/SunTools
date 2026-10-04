# 📘 SunGo DB Designer — User Manual

Version 01.00.00-beta-17 · Lothar TeaM

This manual guides you from a blank canvas to a working database inside your Go application. Background example: a small store — **customers**, **orders**, **products**.

---

## Table of Contents

1. [Installation and Launch](#1-installation-and-launch)
2. [Start Screen](#2-start-screen)
3. [Application Window](#3-application-window)
4. [Tables](#4-tables)
5. [Columns](#5-columns)
6. [Relationships](#6-relationships)
7. [Indexes](#7-indexes)
8. [Validation](#8-validation)
9. [Project Settings](#9-project-settings)
10. [Code Preview](#10-code-preview)
11. [Export](#11-export)
12. [Migrations — Changes After First Export](#12-migrations--changes-after-first-export)
13. [Connecting the Database to Your Application](#13-connecting-the-database-to-your-application)
14. [Keyboard Shortcuts](#14-keyboard-shortcuts)
15. [Common Issues](#15-common-issues)

---

## 1. Installation and Launch

The application window will open. DB Designer runs on port **8788**, and GUI Builder on port **8787**, allowing both to run simultaneously.

Projects are saved as `projects/<Name>.json` next to the executable file.

---

## 2. Start Screen

- **New project name**: enter a name. 💡 If you are creating an interface in GUI Builder for the same application, **use the same name**. Both tools will then export files to a single project directory.
- **SQLite / PostgreSQL / MySQL tiles**: select the database engine.
  - **SQLite**: file on disk, serverless. The best choice for desktop applications.
  - **PostgreSQL**: database server, for network applications and multi-user setups.
  - **MySQL / MariaDB**: when required by the server or hosting environment.
- **Open existing**: list of saved projects.

The database engine can be changed later, but it is best to do so before the first export (see [Chapter 12](#12-migrations--changes-after-first-export)).

---

## 3. Application Window

```
┌──────────────── topbar: project · undo/redo · zoom · Code · Save · Load · Export ─────┐
│ Palette        │                Canvas (diagram)                     │  Properties       │
│ • New table    │   ┌───────────┐          ┌────────────┐             │  (project, table  │
│ • Relationships│   │ customers │──────────<│   orders   │             │   or column)      │
│ • Table list   │   └───────────┘          └────────────┘             │                   │
└──────────── status bar: messages · errors · theme · language · links ─────────────────────┘
```

- **Palette** (left side): tools, relationships, and table list. Clicking a table in the list scrolls the canvas directly to it.
- **Canvas** (center): diagram. **Ctrl + scroll wheel** zooms in/out, and the **Fit** button displays the entire diagram.
- **Properties** (right side) depend on your active selection:
  - empty space → **project** settings,
  - table header → **table** settings,
  - column row → **column** settings.
- **Status bar**: system messages, error counter, theme switch (Classic / SunGo), and language selector (PL/EN).

---

## 4. Tables

### Adding

You have three methods:
- click **New table** in the palette,
- drag **New table** onto the canvas,
- **double-click** an empty space on the canvas.

**Table + timestamps** immediately adds `created` and `updated` columns (`utworzono` / `zmieniono`), which the application populates automatically.

A new table already includes an `id` column (auto-increment primary key), and the cursor is placed in the name field for immediate typing.

### Moving

Grab the table by its **colored header** and drag it. The table snaps to the grid. Holding **Alt** allows precise positioning without grid snapping.

### Table Settings

| Field | Description |
|---|---|
| Table name (SQL) | e.g., `customers`. Only letters, numbers, and `_` are allowed, with no special or accented characters. |
| Go struct name | Empty = name derived from table (`customers` → `Customers`). Type `Customer` if you prefer singular. |
| Comment | Included in the Go code and exported to the database schema (PostgreSQL, MySQL). |
| Color | Header color on the diagram. Used for visual organization only. |

**Delete table**: button at the bottom of the panel or the **Delete** key. The application will issue a warning if other tables have active relationships pointing to it. Those relationships will be deleted along with the table.

---

## 5. Columns

### Quick Edit (table panel)

Each row in the column list is structured as follows:

`🔑` · **name** · **type** · **NN** · `⋯` · `×`

- 🔑: primary key (click to toggle),
- NN: NOT NULL, meaning the column must contain a value,
- ⋯: full column settings,
- ×: delete column.

Pressing **Enter** in the name field moves directly to the next column, enabling full table entry without using a mouse.

Buttons below the list:
- **+ Column** adds a new column,
- **+ id** adds an `id` primary key,
- **+ time** adds `created` and `updated` timestamp columns.

### Data Types

| Type | Intended Use | Go Representation |
|---|---|---|
| `int` | standard integer | `int` |
| `int64` | large integer, primary keys | `int64` |
| `float` | floating-point number | `float64` |
| `decimal` | amounts, prices (digits + fixed decimal places) | `float64` |
| `string` | short text with length limit | `string` |
| `text` | long text without limit | `string` |
| `bool` | boolean (yes / no) | `bool` |
| `datetime` | date and time | `time.Time` |
| `date` | date only | `time.Time` |
| `uuid` | UUID identifier | `string` |
| `json` | JSON document | `string` |
| `blob` | binary data, files | `[]byte` |

💡 A column **without** NOT NULL is generated as a **pointer** in Go (`*string`, `*time.Time`). `nil` represents "no value" (NULL).

### Full Column Settings (⋯)

**In Database:**
- **Primary key**, and for int/int64 types also **Auto-increment**.
- **NOT NULL**: column must contain a value.
- **Unique**: two rows cannot share the exact same value (e.g. email).
- **Default value (SQL)**, e.g., `0`, `'new'`, `CURRENT_TIMESTAMP`. Text values must be enclosed in single quotes.
- **Automatically set timestamp** (datetime/date only):
  - *on creation*: set automatically during `Create`,
  - *on every update*: set automatically during `Create` and on every `Update`.
- **Comment**.

**Foreign Key** and **Validation** are detailed in subsequent sections.

The **↑ ↓** arrow buttons adjust column order. The generated Go struct preserves this exact sequence.

The **← table …** link at the top of the panel returns to table settings.

---

## 6. Relationships

### Drawing

1. In the palette, click the desired relationship type. An orange tooltip indicator will appear on the canvas.
2. Click the **parent** table, representing the "one" side (e.g. `customers`).
3. Click the **child** table, representing the "many" side (e.g. `orders`).

Press **Esc** to cancel drawing.

| Type | Generated Structure | Example |
|---|---|---|
| **1:N** (one-to-many) | A new foreign key column in the child table, e.g., `customers_id` | One customer has many orders |
| **1:1** (one-to-one) | Same as above, but the foreign key column is set to unique | User ↔ Profile |
| **N:M** (many-to-many) | A new **junction table** containing two foreign keys | An order contains many products; a product belongs to many orders |

The junction table (e.g., `orders_products`) features a dashed border and an **N:M** badge. Custom columns can be appended to it, such as `quantity` or `price_at_purchase`.

💡 A relationship can target its own table (e.g., parent category → subcategories). The foreign key column is optional in this setup.

### Diagram Line Indicators

- solid stroke `|` at the parent table denotes "one",
- split line ⋲ ("crow's foot") at the child table denotes "many",
- circle `o` indicates that the relationship is optional (foreign key can be NULL).

Clicking a relationship line selects the underlying foreign key column.

### Parent Row Deletion Behavior

In foreign key column settings (⋯ → **Foreign Key**), specify the action triggered upon deleting a parent record:

| Option | Action when deleting a customer with active orders |
|---|---|
| **restrict** (default) | Database blocks deletion of the customer record |
| **CASCADE** | Associated customer orders are automatically deleted |
| **SET NULL** | Orders remain; the `customers_id` field is set to NULL (column must allow NULLs) |

Foreign keys can also be mapped manually for any column via the **Points to table** field.

---

## 7. Indexes

Indexes accelerate database lookup queries. It is recommended to index columns frequently used in filter operations, such as `status` or `last_name`.

1. Select a table and click **+ Index**.
2. Input an index name (e.g., suggested name `ix_orders_status`).
3. Select target columns. Composite indexes can include multiple columns.
4. Optionally check **unique** if combination values must remain unique.

Single unique columns do not require manual index creation—checking **Unique** in column settings achieves this automatically.

---

## 8. Validation

Column settings (⋯) → **Validation**. Rules configured here compile into a generated `Validate()` method for each table model.

| Rule | Supported Types | Example |
|---|---|---|
| Required | all types | First name cannot be empty |
| Minimum length | text | Password: min 8 characters |
| Maximum length | `string`, auto-set from column length | `varchar(100)` → max 100 characters |
| Email address | text | `jan@example.com` ✔, `jan@` ✘ |
| Pattern (regex) | text | Postal code: `^[0-9]{2}-[0-9]{3}$` |
| Min / Max | numeric | Rating scale from 1 to 5 |

Validation failures return structured field errors with descriptions (e.g., `email: invalid email address`), simplifying form error handling.

---

## 9. Project Settings

Access project settings by clicking any empty space on the canvas.

**Database Configuration:**
- **Database Engine**: SQLite / PostgreSQL / MySQL.
- **SQLite Driver**: `modernc` (pure Go implementation, recommended, CGO-free) or `mattn` (requires GCC/CGO).
- **Database / File Name**: e.g., `store` → `store.db` file or `store` database name on server.
- **Connection (DSN)**: Full database connection string (including PostgreSQL credentials). If empty, defaults to suggested address.

**Go Code Settings:**
- **Source directory**: Defaults to `SRC`, matching GUI Builder structure.
- **Go Module**: Leave empty by default. Automatically detected from `go.mod`.
- **validate / gorm tags**: Appends additional tag annotations to generated Go structs.

**Project Check**: Inspector listing errors and notices. Clicking an item navigates to the error location; problematic tables and columns are highlighted red on the canvas. Projects containing errors (⛔) will block code export. Notices (⚠) are purely informational.

---

## 10. Code Preview

Clicking **Code** in the topbar previews all generated source files exactly as they will be written to disk. The left pane provides a file tree, while the right pane renders formatted code syntax highlighting. The **Copy** button copies active file contents to the clipboard.

Reviewing generated code before initial export is recommended—specifically `handlersDb.go` and `db/doc.go`.

---

## 11. Export

1. Click **Export…**.
2. Select the **parent directory** (use the same parent directory designated in GUI Builder). The application persists this path for future exports.
3. Review the export summary:
   - **Target File Path**: `<folder>/<Project>/SRC/db/` + `SRC/handlersDb.go`,
   - **Go Import Path**: Derived package path and resolution source (`go.mod`, project settings, or project name),
   - **Migrations**: Indicates initial export setup vs. incremental migration creation,
   - **Schema changes** and structural warnings.
4. Click **Export**.

Generated directory structure:

```
<Project>/SRC/
├── handlersDb.go          ← application glue code
└── db/
    ├── models_gen.go      ← Go structs
    ├── crud_gen.go        ← CRUD operations
    ├── validate_gen.go    ← Validate() implementation
    ├── db_gen.go          ← Open, Migrate, transactions
    ├── doc.go             ← package documentation
    ├── schema.sql         ← full SQL schema file
    ├── schema.snapshot.json
    └── migrations/0001_…_init.sql
```

⚠️ **Do not edit** `*_gen.go`, `doc.go`, `schema.sql`, or `handlersDb.go` files directly, as they are overwritten on every export. Place custom business logic in separate files inside `package db` (e.g., `SRC/db/queries.go`).

💡 Projects save automatically upon successful code export.

---

## 12. Migrations — Changes After First Export

Modifying the diagram and re-exporting generates an incremental file inside `migrations/` (e.g., `0002_…_customers_added_phone.sql`), recording **only schema delta changes**.

| Action in Designer | Database Execution Result |
|---|---|
| Rename column or table | Renames entity while **preserving existing data** |
| Add column, table, index | Creates new schema object |
| Delete column or table | Drops object **permanently deleting stored data** (warned in export modal) |
| Change type, NOT NULL, default value | Alters column definition. In SQLite, table is rebuilt and migrated |

Upon application boot (`dbInit()`), pending migrations execute automatically inside individual transactions. Executed migration state is stored in the `schema_migrations` meta-table to prevent re-execution.

⚠️ Adding a new **NOT NULL column without a default value** to a populated table will fail because existing rows cannot satisfy the constraint. Assign a default value or uncheck NOT NULL.

### Resetting Migration History

Checking the reset option in the export window removes existing migration files and consolidates schema history into a fresh `0001_..._init.sql`. Recommended during:
- Early development and rapid prototyping phases,
- Switching database engines (e.g., SQLite → PostgreSQL). The designer auto-selects this box on engine changes.

*Note: Existing databases must be re-initialized manually (e.g., deleting local `.db` file).*

---

## 13. Connecting the Database to Your Application

Following initial export, run **Import all** inside SunGo Project Manager (or execute `go mod tidy` in terminal) to download database drivers.

### A) WebUI Projects from GUI Builder (`backend.go`)

**In `main()`**, prior to `http.ListenAndServe`:

```go
if err := dbInit(); err != nil {
	log.Fatal(err)
}
defer dbClose()
```

**In `handleEvent`**, immediately following `req` decoding:

```go
if resp, ok := dbEvent(req.Event, req.Data); ok {
	w.Header().Set("Content-Type", "application/json")
	json.NewEncoder(w).Encode(resp)
	return
}
```

**In Frontend (`app.js`):**

```js
// Fetch paginated list
const r = await sendToBackend("db.customers.list", { limit: 50, offset: 0 });
if (r.ok) console.log(r.rows, r.total);

// Create record
const n = await sendToBackend("db.customers.create", {
    first_name: "Jan", email: "jan@example.com", birth_date: "1990-05-17"
});
if (!n.ok) console.log(n.error, n.fields); // validation errors

// Get single record, edit, and delete
const k = (await sendToBackend("db.customers.get", { id: 1 })).row;
k.first_name = "Janek";
await sendToBackend("db.customers.update", k);  // Pass COMPLETE object — see warning below
await sendToBackend("db.customers.delete", { id: 1 });

// Query relations via foreign key
await sendToBackend("db.orders.by_customers_id", { customers_id: 1 });
```

Available RPC events:

| Event Name | Payload Sent | Payload Received |
|---|---|---|
| `db.<table_name>.list` | `{limit, offset}` | `{ok, rows, total}` |
| `db.<table_name>.get` | `{id}` | `{ok, row}` |
| `db.<table_name>.create` | Field object | `{ok, row}` containing auto-generated `id` |
| `db.<table_name>.update` | `{id}` + field object | `{ok, row}` |
| `db.<table_name>.delete` | `{id}` | `{ok}` |
| `db.<table_name>.count` | none | `{ok, count}` |
| `db.<table_name>.by_<fk_column>` | `{<fk_column>: …}` | `{ok, rows}` |

⚠️ **`update` persists full record states.** Missing properties in payload payloads will clear destination values in the database. Always fetch records first (`get`), modify desired fields, and return full objects. *Exception: "Timestamp on creation" fields are protected from `update` mutations.*

Errors return formatted payloads: `{ok: false, error: "…", fields: [...]}`. Input values from HTML `<input type="date">` and `datetime-local` elements process directly.

Custom events (lacking the `db.` namespace) bypass database handlers and function normally.

### B) Fyne Projects from GUI Builder (`handlers.go`)

```go
// inside onReady():
if err := dbInit(); err != nil {
	log.Fatal(err)
}

// at the start of handleEvent:
if resp, ok := dbEvent(event, data); ok {
	// e.g., assign resp["rows"] to UI components
	return
}
```

### C) Direct Go Integration (Without GUI Builder)

```go
if err := dbInit(); err != nil {
	log.Fatal(err)
}
defer dbClose()
ctx := context.Background()

k := db.Customers{FirstName: "Jan", Email: "jan@example.com"}
if err := k.Validate(); err != nil {
	fmt.Println(err) // validation: email: invalid email address
}
dbStore.CreateCustomers(ctx, &k)             // k.ID is auto-populated
list, _ := dbStore.ListCustomers(ctx, 50, 0) // query first 50 records
single, err := dbStore.GetCustomers(ctx, k.ID)
if errors.Is(err, db.ErrNotFound) { /* handle missing record */ }
```

Reference IDE autocompletion (`dbStore.` + Ctrl+Space) or consult `SRC/db/doc.go` for available method listings.

### Database Location

- **SQLite**: Local `<name>.db` file created in current working directory.
- **PostgreSQL / MySQL**: Host endpoints defined in project DSN configuration.

Runtime connection parameters can be overridden without re-exporting by populating the **`DB_DSN`** environment variable.

---

## 14. Keyboard Shortcuts

| Shortcut | Action |
|---|---|
| **Ctrl+Z** / **Ctrl+Y** | Undo / Redo |
| **Ctrl+S** | Save project |
| **Delete** | Delete selected table or column |
| **Esc** | Cancel relationship drawing / close modal / deselect |
| **Enter** (in column name) | Commit and jump to next column |
| **Ctrl + Scroll Wheel** | Canvas zoom in / zoom out |
| **Alt** + Drag | Bypass grid snapping when repositioning |
| Canvas Double-Click | Create new table at cursor position |
| Table / Column Double-Click | Direct inline name editing |

---

## 15. Common Issues

**"Export failed: project contains errors"**
Click an empty canvas area to open Project Settings and review the **Project Check** diagnostic list. Clicking an entry redirects focus to the offending entity.

**Import statement in `handlersDb.go` contains incorrect module name (`Store/SRC/db` instead of module name from `go.mod`)**
Export was executed prior to running `go mod init`. Re-exporting resolves the path by re-reading `go.mod`.

**"previous migrations are for sqlite database, but project now uses postgres"**
Database engine was changed. Check **Reset migrations from scratch** in export parameters and re-initialize target database schema.

**Migration fails on startup with NOT NULL constraint error**
A NOT NULL column without a default value was introduced to a populated table. Assign a **Default Value** or uncheck NOT NULL and re-export.

**MySQL: Dates return as text strings or `update` returns "not found"**
Connection string lacks required driver options (`parseTime=true` or `clientFoundRows=true`). Default DSN templates include these parameters; ensure custom connection strings keep them.

**MySQL: "column of type text cannot be in index/unique"**
Database engine restriction. Change column type from `text` to `string` (VARCHAR with explicit length).

**SQLite: "database is locked"**
Generated packages utilize pooled single-connection routines, preventing locks in standard operation. When opening manual concurrent connections, reuse `dbConn` exported by `handlersDb.go`.

**Manual edits in `crud_gen.go` vanished**
Files suffixed with `*_gen.go` regenerate on every export step. Place custom SQL procedures in separate source files within `SRC/db/` (e.g., `queries.go`).

---

Submit inquiries and issue reports at **[forum.lothar-team.pl](https://forum.lothar-team.pl)** 🐹