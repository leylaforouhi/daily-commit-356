from datetime import datetime
from pathlib import Path

def create_invoice():
    print("\n===== INVOICE GENERATOR =====")

    customer = input("Customer name: ").strip()

    if not customer:
        print("Customer name is required.")
        return

    try:
        count = int(input("Number of products: "))

        if count <= 0:
            print("Enter a positive number.")
            return

        items = []
        subtotal = 0

        for i in range(count):
            print(f"\nProduct {i + 1}")

            name = input("Product name: ").strip()
            price = float(input("Price: "))
            quantity = int(input("Quantity: "))

            if not name or price < 0 or quantity <= 0:
                print("Invalid product details.")
                return

            total = price * quantity
            subtotal += total

            items.append((name, quantity, price, total))

        tax = subtotal * 0.10
        grand_total = subtotal + tax

        invoice_id = datetime.now().strftime("%Y%m%d%H%M%S")

        invoice = [
            "========== INVOICE ==========",
            f"Invoice ID: {invoice_id}",
            f"Date: {datetime.now():%Y-%m-%d}",
            f"Customer: {customer}",
            "-----------------------------"
        ]

        for name, quantity, price, total in items:
            invoice.append(
                f"{name} | {quantity} x {price:.2f} = {total:.2f}"
            )

        invoice.extend([
            "-----------------------------",
            f"Subtotal: {subtotal:.2f}",
            f"Tax (10%): {tax:.2f}",
            f"Total: {grand_total:.2f}",
            "============================="
        ])

        output = "\n".join(invoice)

        Path(f"invoice_{invoice_id}.txt").write_text(
            output,
            encoding="utf-8"
        )

        print("\n" + output)
        print("\nInvoice saved successfully!")

    except ValueError:
        print("Please enter valid numeric values.")

if __name__ == "__main__":
    create_invoice()
