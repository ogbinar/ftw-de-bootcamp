# 🧭 How to Connect Tableau Desktop to the FTW ClickHouse Server

### ✅ Requirements

* **Tableau Desktop 2020.4 or later**
* **ClickHouse 20.7 or later**
* **ClickHouse JDBC Driver (v0.9.X)**
* **ClickHouse Tableau Connector (.taco)**

---

## ⚙️ Step 1 – Install the JDBC Driver

1. Download the latest 0.9.X JDBC driver from the ClickHouse GitHub page.
2. Copy the file `clickhouse-jdbc-0.9.X-all-dependencies.jar` to:

   ```
   C:\Program Files\Tableau\Drivers
   ```

   *(Create the folder if it doesn’t exist.)*

---

## ⚙️ Step 2 – Install the Tableau Connector

1. Download `clickhouse-jdbc.taco` from the official ClickHouse Tableau Connector Releases page.
2. Copy it to:

   ```
   C:\Users\<YourWindowsUser>\Documents\My Tableau Repository\Connectors
   ```

---

## 🚀 Step 3 – Connect from Tableau Desktop

1. Open **Tableau Desktop**.

2. Go to **Connect → To a Server → More… → ClickHouse JDBC by ClickHouse, Inc.**

3. Enter the following details:

   | Field             | Value          |
   | ----------------- | -------------- |
   | **Host / Server** | `54.87.106.52` |
   | **Port**          | `8123`         |
   | **Username**      | `ftw_user`     |
   | **Password**      | `ftw_pass`     |

4. Click **Sign In**.

5. Choose your database or schema, then start building visualizations.

---

## 🔄 Step 4 – Select Connection Type

* **Live** – Recommended for large or real-time datasets. Queries run directly on ClickHouse.
* **Extract** – Optional for offline or faster local use. Data is stored in a `.hyper` file and refreshed manually or on schedule.

---

### 🧩 Notes

* Restart Tableau if the ClickHouse connector doesn’t appear after installation.
* Ensure both the `.taco` and `.jar` files are in their correct folders.
* If you encounter connection errors, confirm firewall access and credentials.
