class Product:
    def __init__(self, pid, name, price, stock):
        self.id = pid
        self.name = name
        self.price = price
        self.stock = stock


class Cart:
    def __init__(self):
        self.items = []  

    def add(self, product, qty):
        if qty <= 0:
            print("Quantity must be greater than zero.")
            return

        if qty > product.stock:
            print(f"Only {product.stock} left in stock.")
            return

        # If product already exists in cart, update quantity
        for item in self.items:
            if item['product'].id == product.id:
                item['qty'] += qty
                print(f"Updated {product.name} to x{item['qty']} in cart.")
                return

        self.items.append({'product': product, 'qty': qty})
        print(f"Added {product.name} x{qty}")

    def view(self):
        if not self.items:
            print("Cart empty")
            return None

        print("\n--- Your Cart ---")
        total = 0

        for i in self.items:
            p, q = i['product'], i['qty']
            cost = p.price * q
            total += cost
            print(f"{p.name:<10} x{q:<3} @ ₹{p.price:>6.2f} = ₹{cost:>8.2f}")

        print("-----------------------------------")
        print(f"Subtotal: ₹{total:>25.2f}")
        return total

    def discount(self, total):
        if total > 50000:
            discount_amount = total * 0.10
            final_total = total - discount_amount
            print("10% discount applied!")
            print(f"Discount: ₹{discount_amount:>25.2f}")
            print(f"Final Total: ₹{final_total:>24.2f}")
            return final_total
        return total


# --- Main Program ---

products = [
    Product(101, "Laptop", 50000, 5),
    Product(102, "Mouse", 800, 15),
    Product(103, "Keyboard", 1500, 10)
]

cart = Cart()

while True:
    print("\n--- Menu ---")
    print("1. View Products  2. Add Item  3. View Cart  4. Checkout  5. Exit")
    ch = input("Choice: ")

    if ch == '1':
        print("\n--- Available Products ---")
        for p in products:
            print(f"ID:{p.id:<3} {p.name:<10} Price: ₹{p.price:<6} Stock:{p.stock}")

    elif ch == '2':
        try:
            pid = int(input("Enter Product ID: "))
            qty = int(input("Enter Quantity: "))

            p = next((x for x in products if x.id == pid), None)

            if p is None:
                print("Product not found.")
            else:
                cart.add(p, qty)

        except ValueError:
            print("Invalid input. ID and Quantity must be numbers.")

    elif ch == '3':
        cart.view()

    elif ch == '4':
        total = cart.view()
        if total:
            final_total = cart.discount(total)
            mode = input("Pay mode (card/cash): ").lower().strip()

            if mode in ["card", "cash"]:
                print(f"\nProcessing payment of ₹{final_total:.2f} by {mode}...")

                # Deduct stock
                for item in cart.items:
                    item['product'].stock -= item['qty']

                try:
                    with open("cart.txt", "w") as f:
                        f.write("--- Order Summary ---\n")
                        for i in cart.items:
                            f.write(
                                f"{i['product'].name}, Quantity: {i['qty']}, "
                                f"Price: ₹{i['product'].price * i['qty']:.2f}\n"
                            )
                        f.write(f"Final Amount Paid: ₹{final_total:.2f}\n")

                    print("Order details saved to cart.txt.")
                except IOError:
                    print("Warning: Could not save order details to file.")

                print("Checkout complete! Thank you for your purchase.")
                break
            else:
                print("Invalid payment mode. Checkout aborted.")

    elif ch == '5':
        print("Exiting program. Goodbye!")
        break

    else:
        print("Wrong choice. Please select 1–5.")
