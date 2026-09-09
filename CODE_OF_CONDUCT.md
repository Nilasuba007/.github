# AI-Powered E-Commerce Customer Support Agent

# -------------------------------
# Mock Data
# -------------------------------

orders = {
    "ORD1023": {
        "status": "Out for Delivery",
        "expected": "Today by 7 PM",
        "item": "Shoes",
        "delivered_days": 0
    },
    "ORD1024": {
        "status": "Shipped",
        "expected": "Tomorrow",
        "item": "Watch",
        "delivered_days": 0
    },
    "ORD1025": {
        "status": "Delivered",
        "expected": "Delivered 3 days ago",
        "item": "T-Shirt",
        "delivered_days": 3
    }
}

products = [
    {"name": "Running Shoes", "category": "shoes", "price": 1999},
    {"name": "Casual Shoes", "category": "shoes", "price": 1499},
    {"name": "Smart Watch", "category": "watch", "price": 2999},
    {"name": "Cotton T-Shirt", "category": "tshirt", "price": 799},
    {"name": "Jeans", "category": "jeans", "price": 1599}
]


# -------------------------------
# Memory
# -------------------------------

memory = {
    "order_id": None,
    "customer_name": None,
    "last_item": None
}


# -------------------------------
# Tool 1: Get Order Status
# -------------------------------

def get_order_status(order_id):

    if order_id in orders:

        order = orders[order_id]

        memory["order_id"] = order_id
        memory["last_item"] = order["item"]

        return (
            f"Order {order_id} is {order['status']}. "
            f"Expected: {order['expected']}."
        )

    return "Sorry, I could not find that order."


# -------------------------------
# Tool 2: Return Eligibility
# -------------------------------

def check_return_eligibility(order_id):

    if order_id not in orders:
        return "Order not found."

    order = orders[order_id]

    if order["delivered_days"] <= 7:

        return (
            f"Yes, the {order['item']} from order "
            f"{order_id} is eligible for return within 7 days."
        )

    return "Sorry, the return period has expired."


# -------------------------------
# Tool 3: Search Products
# -------------------------------

def search_products(keyword):

    results = []

    for product in products:

        if keyword.lower() in product["name"].lower():

            results.append(product)

    if len(results) == 0:
        return "No products found."

    answer = "Available products:\n"

    for product in results:

        answer += (
            f"- {product['name']} - "
            f"₹{product['price']}\n"
        )

    return answer


# -------------------------------
# Tool 4: Recommendations
# -------------------------------

def get_recommendations():

    item = memory["last_item"]

    if item is None:
        return "Please tell me an order or product you are interested in."

    if item.lower() == "shoes":

        return (
            "Based on your interest in shoes, "
            "I recommend Running Shoes and Casual Shoes."
        )

    if item.lower() == "watch":

        return "I recommend our Smart Watch."

    if item.lower() == "t-shirt":

        return "I recommend Cotton T-Shirt."

    return "I don't have a recommendation yet."


# -------------------------------
# FAQ
# -------------------------------

def faq(question):

    question = question.lower()

    if "shipping" in question:

        return "Standard shipping usually takes 3-5 days."

    elif "payment" in question:

        return "We support online payment methods."

    elif "return" in question:

        return "Products can generally be returned within 7 days."

    elif "refund" in question:

        return "Refunds are processed after the returned item is verified."

    return "Sorry, I don't know the answer to that question."


# -------------------------------
# Main Chatbot
# -------------------------------

def chatbot():

    print("======================================")
    print(" AI E-Commerce Customer Support Agent ")
    print("======================================")

    print("Type 'exit' to stop.\n")

    while True:

        user = input("You: ")

        if user.lower() == "exit":
            print("Agent: Thank you! Have a nice day.")
            break

        # Order Status
        if "status" in user.lower() or "order" in user.lower():

            order_id = input("Agent: Please enter your Order ID: ")

            print("Agent:", get_order_status(order_id))

        # Return
        elif "return" in user.lower():

            order_id = memory["order_id"]

            if order_id is None:

                order_id = input(
                    "Agent: Please enter your Order ID: "
                )

            print(
                "Agent:",
                check_return_eligibility(order_id)
            )

        # Recommendation
        elif "recommend" in user.lower():

            print(
                "Agent:",
                get_recommendations()
            )

        # Product Search
        elif "product" in user.lower() or "search" in user.lower():

            keyword = input(
                "Agent: What product are you looking for? "
            )

            print(
                "Agent:",
                search_products(keyword)
            )

        # FAQ
        else:

            print(
                "Agent:",
                faq(user)
            )


# -------------------------------
# Run Program
# -------------------------------

if __name__ == "__main__":
    chatbot()
