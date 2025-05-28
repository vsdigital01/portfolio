# Subcontractor Claim Entry System
# This is a Python GUI application that allows a construction company to track & calculate subcontractor claims.


```python
import tkinter as tk
from tkinter import messagebox, ttk
import csv
import os

# Filename for saving claims
FILENAME = "claims.csv"
```


```python
def calculate_and_save():
    try:
        # Retrieve inputs
        name = entry_name.get()
        site = entry_site.get()
        claim = entry_claim.get()
        variations = float(entry_variations.get())
        discount = float(entry_discount.get() or 0)

        # Get historical totals for this subcontractor/site
        previous_payments = get_previous_payments(name, site)
        value_to_date = get_value_to_date(name, site)

        # Perform calculations
        subtotal1 = value_to_date + variations
        subtotal2 = subtotal1 - discount
        retention_percent = 0.05 if retention_var.get() == "5%" else 0.10
        retention = variations * retention_percent
        total_valuation = subtotal2 - retention
        subtotal3 = total_valuation - previous_payments
        vat = subtotal3 * 0.155
        final_total = subtotal3 + vat

        # Save result to CSV
        row = [
            name, site, claim, f"{value_to_date:.2f}", f"{variations:.2f}", f"{subtotal1:.2f}",
            f"{discount:.2f}", f"{subtotal2:.2f}", f"{retention_percent*100:.0f}%", f"{retention:.2f}",
            f"{total_valuation:.2f}", f"{previous_payments:.2f}", f"{subtotal3:.2f}", f"{vat:.2f}", f"{final_total:.2f}"
        ]
        save_to_csv(row)

        messagebox.showinfo("Success", f"Claim for {name} saved.\nFinal Total: {final_total:.2f}")
        clear_fields()

    except ValueError:
        messagebox.showerror("Error", "Please enter valid numeric values for Variations and Discount.")
```


```python
def save_to_csv(row):
    # Append a row to the CSV file, with headers if file does not exist
    file_exists = os.path.isfile(FILENAME)
    with open(FILENAME, mode="a", newline="") as file:
        writer = csv.writer(file)
        if not file_exists:
            writer.writerow([
                "Subcontractor", "Site Code", "Claim #", "Value To Date", "Authorized Variations",
                "Subtotal 1", "Discount", "Subtotal 2", "Retention %", "Retention Value",
                "Total Valuation", "Previous Payments", "Subtotal 3", "VAT", "Final Total"
            ])
        writer.writerow(row)
```


```python
def get_previous_payments(name, site):
    # Sum all previous Final Total amounts for same subcontractor and site
    total = 0
    if os.path.isfile(FILENAME):
        with open(FILENAME, newline="") as file:
            reader = csv.DictReader(file)
            for row in reader:
                if row["Subcontractor"] == name and row["Site Code"] == site:
                    total += float(row["Final Total"])
    return total

def get_value_to_date(name, site):
    # Sum all previous Authorized Variations for same subcontractor and site
    total = 0
    if os.path.isfile(FILENAME):
        with open(FILENAME, newline="") as file:
            reader = csv.DictReader(file)
            for row in reader:
                if row["Subcontractor"] == name and row["Site Code"] == site:
                    total += float(row["Authorized Variations"])
    return total
```


```python
def clear_fields():
    # Reset form inputs
    entry_name.delete(0, tk.END)
    entry_site.delete(0, tk.END)
    entry_claim.delete(0, tk.END)
    entry_variations.delete(0, tk.END)
    entry_discount.delete(0, tk.END)
    retention_var.set("5%")
```


```python
def view_summary():
    # Show saved claims in a new scrollable window
    if not os.path.exists(FILENAME):
        messagebox.showinfo("No Data", "No claims have been saved yet.")
        return

    summary_window = tk.Toplevel(root)
    summary_window.title("Claim Summary")

    frame = tk.Frame(summary_window)
    frame.pack(fill=tk.BOTH, expand=True)

    tree = ttk.Treeview(frame)
    tree.pack(side=tk.LEFT, fill=tk.BOTH, expand=True)

    scrollbar = ttk.Scrollbar(frame, orient=tk.VERTICAL, command=tree.yview)
    scrollbar.pack(side=tk.RIGHT, fill=tk.Y)
    tree.configure(yscrollcommand=scrollbar.set)

    # Load data
    with open(FILENAME, newline="") as file:
        reader = csv.reader(file)
        headers = next(reader)
        tree["columns"] = headers
        tree["show"] = "headings"
        for header in headers:
            tree.heading(header, text=header)
            tree.column(header, width=100, anchor="center")

        for row in reader:
            tree.insert("", tk.END, values=row)
```


```python
# Create main window
root = tk.Tk()
root.title("Subcontractor Claim Entry")

# Labels
tk.Label(root, text="Subcontractor Name:").grid(row=0, column=0, sticky="e")
tk.Label(root, text="Site Code:").grid(row=1, column=0, sticky="e")
tk.Label(root, text="Claim Number:").grid(row=2, column=0, sticky="e")
tk.Label(root, text="Authorized Variations:").grid(row=3, column=0, sticky="e")
tk.Label(root, text="Less Discount:").grid(row=4, column=0, sticky="e")
tk.Label(root, text="Retention:").grid(row=5, column=0, sticky="e")

# Entry fields
entry_name = tk.Entry(root)
entry_site = tk.Entry(root)
entry_claim = tk.Entry(root)
entry_variations = tk.Entry(root)
entry_discount = tk.Entry(root)

entry_name.grid(row=0, column=1)
entry_site.grid(row=1, column=1)
entry_claim.grid(row=2, column=1)
entry_variations.grid(row=3, column=1)
entry_discount.grid(row=4, column=1)

# Retention dropdown
retention_var = tk.StringVar(value="5%")
tk.OptionMenu(root, retention_var, "5%", "10%").grid(row=5, column=1, sticky="ew")

# Buttons
tk.Button(root, text="Calculate & Save", command=calculate_and_save).grid(row=6, column=0, columnspan=2, pady=10)
tk.Button(root, text="View Summary", command=view_summary).grid(row=7, column=0, columnspan=2, pady=5)

# Start GUI loop
root.mainloop()
```


```python

```
