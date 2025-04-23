## Dump data

```sh
pg_dump -h remote_host -U postgres -d mydb -f dump_file.sql
```

## List roles (users and groups)

Access the database and run this command:

```sh
SELECT rolname FROM pg_roles;
```

## Create new role

```sh
CREATE ROLE role_name;
```

# Trigger

## 🧠 Use Cases

- Automatically update a `last_modified` column
- Keep audit logs (e.g., store old data in an `events_log` table)
- Enforce business rules
- Sync data across tables

---

## 🏗️ Basic Components of a Trigger

There are **two main parts**:

1. **Trigger Function** – Written in PL/pgSQL or other languages (this is what runs).
2. **Trigger** – Defines **when** and **on what** the trigger function will run.

---

## ✅ Example: Track Updates in an Audit Table

### 1. Create a table

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name TEXT,
    email TEXT
);
```

### 2. Create an audit table

```sql
CREATE TABLE user_audit (
    id SERIAL PRIMARY KEY,
    user_id INT,
    old_name TEXT,
    old_email TEXT,
    changed_at TIMESTAMPTZ DEFAULT now()
);
```

### 3. Create the trigger function

```sql
CREATE OR REPLACE FUNCTION log_user_update()
RETURNS TRIGGER AS $$
BEGIN
    INSERT INTO user_audit(user_id, old_name, old_email)
    VALUES (OLD.id, OLD.name, OLD.email);
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;
```

> This function runs **before the update finishes**. It saves the old values to the audit table.

### 4. Create the trigger

```sql
CREATE TRIGGER trigger_user_update
BEFORE UPDATE ON users
FOR EACH ROW
EXECUTE FUNCTION log_user_update();
```

---

## 🕹️ Trigger Timing Options

- **BEFORE** – run trigger function before operation
- **AFTER** – run it after the operation
- **INSTEAD OF** – used on views to override the normal action

---

## 🔁 Row vs Statement

- **FOR EACH ROW** – runs once **per row**
- **FOR EACH STATEMENT** – runs once per **query**, no matter how many rows

---

## ⚠️ Pro Tips

- Use `OLD` for previous row values
- Use `NEW` for new row values (in INSERT/UPDATE)
- You can **cancel** operations in `BEFORE` triggers by `RAISE EXCEPTION`

---

Would you like to:
- Try a `BEFORE INSERT` trigger example?
- Set up a real audit trail (full user history)?
- Learn how to debug or disable triggers temporarily?

Let me know what direction you'd like to go!
