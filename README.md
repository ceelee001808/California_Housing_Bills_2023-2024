**Goal:**
My goal was to show - at a glance- the 2023-2024 California housing bills that have the strongest organisational backing (and which face resistance) by turning a raw CSV of data analysis filings into an interactive chart.

**Data Source:**
The CSV file was provided by my research supervisor Professor Clayton Nall from the UCSB Poli-Sci department. The data consists of 178 distinct housing-related bills with 1678 unique organizations either supporting or against certain bills.

**Column	Meaning	Example From CSV File:**
bill.no	= Bill identifier (upper-cased later) EX)AB-1490
org	= Organisation name (trimmed & upper-cased) EX) AIDS Healthcare Foundation
support.lvl	3 = supports 
support.lvl 0 = opposes	
sponsor	1 = sponsor of the bill
sponsor 0 = not a sponsor of the bill
enteredby = 2023-2024	*did not use 

**Pipeline (Databricks + PySpark → Plotly):**
  a) Install dependencies. The very first cell (Cell 0) upgrades Plotly by running pip install --upgrade plotly, ensuring that the latest interactive-charting functions are available.
  b) Load and clean the data. In Cells 1, 3, and 5, the notebook reads the CSV file into a Spark DataFrame, discards metadata columns that are not needed, renames the column support.lvl to the more Python-friendly support_lvl, and begins standardising bill and organisation names.
  c) Normalise text fields.
  Cell 7 converts every bill identifier to uppercase, trims whitespace, and stores the result in a new column called bill_id. It performs the same upper-case–and-trim transformation on organisation names, saving them in a column named org_norm.
  d) Summarise support and opposition. In the first half of Cell 8, the code groups the data by each combination of bill_id and the binary flag support_flag. For every pair, it calculates the number of organisations taking that stance (org_count) and concatenates their names into a newline-separated string (org_list) that will later appear in hover tooltips.
  e) Rank and order the bills. The middle of Cell 8 sorts the resulting summary so that bills with the fewest supporters rise to the top. As a special case, it always places bill SB-4 first, because that bill has been singled out for quick reference.
  f) Visualise the results. Finally, the bottom part of Cell 8 constructs a stacked horizontal bar chart with Plotly. Green bars represent supporting organisations and red bars represent opposing ones. When users hover over a bar, they see the full list of organisations behind that stance. The chart applies a custom colour palette, Plotly’s clean “white” theme, and automatically adjusts its height to fit however many bills appear.

**Possible Next Steps:**
  a) Network graph of coalitions (which orgs co-support the same bills).
  b) Time-series tracking of support as amendments roll in.
  c) Bill classification (subsidies vs regulatory, etc.) to see stance patterns by category.






