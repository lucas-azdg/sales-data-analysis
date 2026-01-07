import csv

# --- LEER CSV ---
sales = []
with open("sales.csv", newline="", encoding="utf-8") as file:
    reader = csv.DictReader(file, delimiter=",")
    for row in reader:
        quantity = int(row["quantity"].strip())
        price = float(row["price"].strip())
        total = quantity * price
        sale = {
            "date": row["date"].strip(),
            "product": row["product"].strip(),
            "salesperson": row["salesperson"].strip(),
            "quantity": quantity,
            "price": price,
            "total": total
        }
        sales.append(sale)

# --- MÉTRICAS ---
total_sales = sum(sale["total"] for sale in sales)

# Ventas por producto
sales_by_product = {}
for sale in sales:
    sales_by_product[sale["product"]] = sales_by_product.get(sale["product"], 0) + sale["total"]

# Ventas por vendedor
sales_by_salesperson = {}
for sale in sales:
    sales_by_salesperson[sale["salesperson"]] = sales_by_salesperson.get(sale["salesperson"], 0) + sale["total"]

# Producto más vendido
most_sold_product = max(sales_by_product, key=sales_by_product.get)

# --- DASHBOARD ---
def print_bar(label, value, scale=50):
    """Imprime barra proporcional en consola"""
    bar_length = int(value / max_value * scale)
    bar = "█" * bar_length
    print(f"{label:15} | {bar} {value:.2f}")

print("\n=== SALES DASHBOARD ===")
print(f"Total Sales: {total_sales:.2f}")
print(f"Most Sold Product: {most_sold_product}\n")

max_value = max(max(sales_by_product.values()), max(sales_by_salesperson.values()))

print("Sales by Product:")
for product, total in sales_by_product.items():
    print_bar(product, total)

print("\nSales by Salesperson:")
for person, total in sales_by_salesperson.items():
    print_bar(person, total)
