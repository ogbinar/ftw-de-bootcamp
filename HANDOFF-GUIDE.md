# 📦 FTW DE Bootcamp – **Capstone Handover Pack**

*A lean, student-friendly version of a real enterprise handover document.*

Your goal:
Deliver your capstone **as if a real client needs to understand it AND continue the project after your access is removed**.

Anyone opening your handover should be able to:

* Understand the business problem
* Know what data you used
* Re-run the pipeline
* Understand your models and data model
* Open the Tableau dashboard and see the insights
* See limitations and next steps

All using **just this one folder**.

---

## 1️⃣ Final Folder Structure (This is Required)

Name your submission folder:

> **`[TEAM NAME] – [PROJECT NAME] – Capstone Handover`**

Inside:

```text
📁 [TEAM NAME] – [PROJECT NAME] – Capstone Handover
│
├── 📄 00_PROJECT_OVERVIEW.docx
├── 📁 01_DATA_SOURCES
├── 📁 02_PIPELINE_AND_CODE
├── 📁 03_DATA_MODEL
├── 📁 04_ANALYSIS_AND_DASHBOARDS
├── 📁 05_LIMITATIONS_AND_NEXT_STEPS
└── 📁 06_PRESENTATION
```

Each section below explains what goes inside.

---

## 2️⃣ What Goes in Each Folder

---

### 📄 00_PROJECT_OVERVIEW.docx

*(max 1–2 pages, written like an executive brief)*

Include:

### **1. Problem & Business Context**

* What problem were you solving?
* Who is the “customer” (industry + role)?
* Why does this matter?

### **2. Data & Approach (High-Level)**

Example phrasing:

> “We ingest raw datasets using dlt → transform with dbt → store in ClickHouse → visualize insights in Tableau.”

List **main datasets only**, not every column.

### **3. Key Insights (3–5 bullets)**

Plain English findings that matter.

### **4. Recommendations (3–5 bullets)**

Realistic “what should we do next” actions a manager can understand.

---

### 📁 01_DATA_SOURCES

Minimum required:

### **1. `Source_Description.xlsx`**

Include columns like:
| System | Table/File | Description | Freshness | Owner | Notes |

### **2. `Sample_Raw_Data.(csv/xlsx)`**

≤ 100 anonymized rows of representative raw data.

Optional (but great):

* `Source_ERD.png` – simple diagram showing original relationships.

---

### 📁 02_PIPELINE_AND_CODE

This is your data engineering evidence.

### Must include:

### **1. `Environment_Setup.txt`**

Should answer:

* Python version?
* Tools used (dlt, dbt, ClickHouse, Tableau)?
* Required installations?
* Where config lives (use placeholders, no secrets)?

---

### **2. `RUN_ORDER.txt` (most important file here)**

This must show the exact steps to rebuild your pipeline **from raw data → transformed tables**.

Example:

```txt
1. Install dependencies:  pip install -r requirements.txt
2. Run ingestion via dlt: python ingestion/ingest_sales.py
3. Start ClickHouse server (Docker or local)
4. Run transformations:  dbt run --select staging+ marts+
5. Validate models:      dbt test
6. Open Tableau and refresh extracts from mart tables
```

It must be deterministic.

---

### **3. Code folders**

Your folder should look like:

```text
📁 02_PIPELINE_AND_CODE
│
├── 📁 ingestion/            # dlt scripts or notebooks
├── 📁 transformation/       # dbt project or SQL transforms
└── 📁 utilities/            # helpers (optional)
```

Every script must have:

* A short header comment
* Inputs + outputs described
* No secrets or hardcoded personal paths

---

### 📁 03_DATA_MODEL

This shows your analytical engineering maturity.

### Minimum required:

### **1. `Star_Schema_ERD.png`**

A simple diagram showing:

* Fact tables
* Dimension tables
* Primary/foreign keys
* How everything joins

No need for Miro-level styling — clarity is the priority.

---

### **2. `Table_Dictionary.xlsx`**

One sheet per final mart table *or* grouped in sections.

Columns should be:

| Column | Data Type | Description | Business Rule / Notes |

Focus on final transformed tables used in Tableau.

---

### 📁 04_ANALYSIS_AND_DASHBOARDS

This is the DA side of your work.

### Must include:

### **1. Tableau Dashboard (Packaged Workbook)**

Place inside:

```text
📁 04_ANALYSIS_AND_DASHBOARDS
│
├── 📁 tableau/
│   ├── [ProjectName]_Dashboard_v1.twbx
│   └── Dashboard_Screenshot.png
└── 📄 Dashboard_Notes.txt
```

**Important:**

* Export as **.twbx** (packaged workbook)
* Convert all live connections → **Extracts**
* Ensure the dashboard is **standalone**
* No links to your machine, no database dependencies

---

### **2. `Dashboard_Notes.txt`**

Fill out one block per dashboard:

```txt
DASHBOARD NAME: Sales Overview
TOOL: Tableau
DATA SOURCE: mart.fact_sales_joined (ClickHouse extract)
MAIN QUESTIONS:
  - How are sales trending?
  - Which categories drive growth?
HOW TO READ:
  - Adjust date range using filter on top right
CAVEATS:
  - Refunds not included → metrics show gross revenue only
```

---

### 📁 05_LIMITATIONS_AND_NEXT_STEPS

This is where you show critical thinking.

Create:

### **1. `Limitations_and_Risks.docx`**

Include:

### **A. Data Limitations**

* Missing fields
* Sample bias
* Low granularity
* Short date ranges

### **B. Technical Limitations**

* Manual steps
* Local paths
* Hardcoded values
* Pipeline fragility

### **C. Future Enhancements (3–5 bullets)**

Examples:

* Add incremental models
* Improve data quality tests
* Build additional dashboards
* Add forecasting or ML

---

### 📁 06_PRESENTATION

Include:

* `Final_Presentation.pptx`
* Any extra visual assets

The deck must stand alone:

* Problem
* Data
* Pipeline
* Data model
* Dashboard
* Insights
* Recommendations
* Limitations

---

## 3️⃣ Non-Negotiable Checklist (You MUST say “yes” to all)

### ✔ Standalone Project?

* [ ] Pipeline can be run using only this folder
* [ ] No credentials included
* [ ] No paths pointing to your machine
* [ ] No external dependency (e.g., your local db)

### ✔ Dashboard Correctness?

* [ ] Tableau file is `.twbx`
* [ ] All connections → Extracts
* [ ] Data fully packaged
* [ ] Dashboard opens on another device

### ✔ Clarity for a Manager?

* [ ] 00_PROJECT_OVERVIEW is clear, simple, concise
* [ ] Insights and recommendations are readable in < 3 minutes

### ✔ Clarity for a New Analyst?

* [ ] Dashboards documented
* [ ] Data model understandable

### ✔ Clarity for a New Data Engineer?

* [ ] RUN_ORDER is complete
* [ ] Code structured and labeled
* [ ] Transformations reproducible

If any answer is “no,” fix it before submission.

---

# 🧩 **Creating a Standalone Tableau Dashboard & Deploying to Tableau Public**

## **1. Creating a Standalone Tableau Dashboard**

A standalone Tableau dashboard means the workbook contains **all required data**, **no external connections**, and can be opened by anyone using **Tableau Desktop** or **Tableau Reader**.

### **1.1 Convert All Data Sources to Extracts**

Tableau Public and standalone workbooks cannot use live connections.

**Steps:**

1. Open the workbook.
2. In the **Data** pane, right-click the data source → **Extract Data…**
3. (Optional) Apply filters or aggregation.
4. Click **Extract**.

> Result: The entire dataset is embedded inside the workbook.

---

### **1.2 Save as a Packaged Workbook (.twbx)**

A `.twbx` file bundles:

* the dashboard
* the data extract
* images
* custom shapes
* metadata
* calculations
* local data files

**Steps:**

1. Go to **File → Save As**
2. Choose **Tableau Packaged Workbook (.twbx)**
3. Save the file.

> This creates a fully portable workbook.

---

### **1.3 Ensure No Local Paths or External Dependencies**

Remove anything that will break when opened by someone else.

Do **not** use:

* Local Excel/CSV paths
* Network drives
* Published database sources
* Live SQL connections

Replace all with **Extracts** embedded in the `.twbx`.

---

### **1.4 Test Your Standalone Workbook**

1. Move the `.twbx` to another folder.
2. Disconnect from VPN / databases.
3. Open using **Tableau Reader** or **Tableau Desktop**.

If it opens without errors, it’s truly standalone.

---

---

## **2. Deploying the Dashboard to Tableau Public**

Tableau Public is a **free hosting platform** with important constraints.
You can upload your workbook if the following are true:

### **2.1 Tableau Public Only Accepts EXTRACTS**

The platform **does not support**:

* Live database connections
* SQL connections
* Snowflake, Postgres, SQL Server, MySQL
* Local file paths

> Uploading with a live connection will fail or the data will disappear.

---

### **2.2 Data Becomes Public**

Tableau Public = all data is **publicly visible** and **downloadable**.

Anyone can:

* Download your workbook
* Access the underlying data
* See all fields and rows

**Never upload sensitive, proprietary, or NDA-covered data.**

---

### **2.3 File Size Limits**

For free users:

* **10 MB** max `.twbx` file
* Larger limits for Creator accounts (varies)

Reduce size by:

* Filtering rows
* Removing unused fields
* Aggregating data
* Compressing images
* Using CSV instead of Excel when needed

---

### **2.4 How to Publish**

1. Log in to **Tableau Public**
2. Go to **Profile → Upload a Workbook**
3. Upload your `.twbx`
4. Confirm the dashboard renders correctly

---

### **2.5 Validate After Publishing**

Test on multiple devices:

* Desktop
* Mobile
* Tablet

Confirm:

* No missing data
* No broken sheets
* Filters and actions work properly

---

---

## **3. Final Checklist**

### ✔ **Standalone Workbook Validation**

* [ ] All data connections converted to **Extracts**
* [ ] Saved as **.twbx**
* [ ] No local file paths
* [ ] No live connections to databases
* [ ] No published data sources required
* [ ] Workbook opens on a machine without VPN or database access
* [ ] All shapes, images, icons embedded

---

### ✔ **Tableau Public Compatibility**

* [ ] Data is safe to be **publicly downloadable**
* [ ] File size is under Tableau Public limits
* [ ] No sensitive columns remain (emails, IDs, codes, prices)
* [ ] Extract contains only necessary fields
* [ ] Dashboard renders correctly after publishing

---

### ✔ **Deployment Review**

* [ ] Dashboard tested on multiple devices
* [ ] Filters, tooltips, parameters working
* [ ] Story points (if any) linked properly
* [ ] Customer briefed about public visibility
* [ ] Final `.twbx` uploaded successfully

---

# ✅ **How to Host a Tableau Public Dashboard on Your Website**

There are **two ways**:

1. **Embed the dashboard directly** (most common)
2. **Link to your Tableau Public profile** (simple but not embedded)

Let’s focus on the **embed**, since that’s what websites use.

---

## 🥇 **Method 1 — Embed the Dashboard on Your Website**

### **Step 1 — Publish your dashboard to Tableau Public**

If you haven’t yet:

1. **Open Tableau Desktop**
2. File → **Save to Tableau Public**
3. Log in → Publish

---

### **Step 2 — Get the Embed Code**

1. Go to your dashboard on Tableau Public
2. Click **Share** (top-right)
3. Copy the **Embed Code (iframe)**

It looks like this:

```html
<iframe
  src="https://public.tableau.com/views/YourWorkbookName/YourDashboardName?:embed=yes&:display_count=yes"
  width="100%"
  height="800px"
  frameborder="0">
</iframe>
```

---

### **Step 3 — Paste into your website**

Depending on your site, paste the embed code into:

* **HTML block** (WordPress, Wix, Squarespace)
* **index.html** (static site)
* **React/Vue component** (inside `dangerouslySetInnerHTML` or similar)

Example for a simple HTML page:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Dashboard</title>
  </head>
  <body>
    <h1>My Tableau Dashboard</h1>

    <iframe
      src="https://public.tableau.com/views/YourWorkbookName/YourDashboardName?:embed=yes&:display_count=yes"
      width="100%"
      height="800"
      frameborder="0">
    </iframe>

  </body>
</html>
```

---

## 🥈 Method 2 — Embed with JavaScript API (optional, advanced)

If you want:

* Dynamic resizing
* Passing parameters
* Button interactions
* Filtering from your website

Use Tableau’s JS API.

Load the library:

```html
<script src="https://public.tableau.com/javascripts/api/tableau.viz.v1.js"></script>
```

Add a `div` placeholder:

```html
<div id="tableauViz" style="width: 100%; height: 800px;"></div>
```

Initialize it:

```html
<script>
  var viz;

  function initViz() {
    var container = document.getElementById("tableauViz");
    var url = "https://public.tableau.com/views/YourWorkbookName/YourDashboardName";
    var options = {
      hideTabs: true,
      width: "100%",
      height: "800px"
    };
    viz = new tableau.Viz(container, url, options);
  }

  window.onload = initViz;
</script>
```

This gives you more control over events and filters.

---

## 💡 Pro Tips

### ✔ **Make the iframe responsive**

Wrap it in a container:

```html
<div style="position: relative; padding-bottom: 80%; height: 0; overflow: hidden;">
  <iframe
    src="https://public.tableau.com/views/YourWorkbookName/YourDashboardName?:embed=yes"
    style="position: absolute; top:0; left: 0; width: 100%; height: 100%;"
    frameborder="0">
  </iframe>
</div>
```

---

### ✔ **Remove the Tableau toolbar**

Add this to the URL:

```
?:showVizHome=no&:toolbar=no
```

---

### ✔ **Auto-fit height**

Use `height: 100vh`:

```html
<iframe src="..." style="height: 100vh; width: 100%;"></iframe>
```

---

## 🧪 Troubleshooting

### ❌ Dashboard not loading?

Enable these:

* Your site must allow **iframes**
* Make sure your embedding isn't blocked by mixed HTTP/HTTPS
* Tableau Public dashboard must be **public**, not private

---

### ❌ It looks blurry?

Increase iframe height in px:

```html
height="1500"
```

---

### ❌ Cannot scroll?

Add:

```html
style="overflow:auto;"
```

---

### 🎉 Summary (copy/paste)

> **To host a Tableau Public dashboard on your website:**
>
> 1. Publish dashboard to Tableau Public
> 2. Click **Share → Copy Embed Code**
> 3. Embed the iframe into your website’s HTML
> 4. (Optional) Use Tableau JS API for dynamic controls

