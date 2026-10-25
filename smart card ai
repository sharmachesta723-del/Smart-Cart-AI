
import streamlit as st

st.set_page_config(
    page_title="SmartCart AI",
    page_icon="🛒",
    layout="centered"
)

st.markdown("""
<style>
.stApp {
    background: linear-gradient(180deg, #f0fff4 0%, #ffffff 50%);
}
h1 { color: #166534; }
div.stButton > button[kind="primary"] {
    background: #15803d;
    border-radius: 10px;
}
</style>
""", unsafe_allow_html=True)

if "items" not in st.session_state:
    st.session_state.items = []

if "budget" not in st.session_state:
    st.session_state.budget = 1500.0

st.title("🛒 SmartCart AI")
st.caption("Shop Freely. Stay Budget-Friendly.")
st.write("Your smart shopping companion, made for students and hostellers.")

budget = st.number_input(
    "💰 Your shopping budget (₹)",
    min_value=1.0,
    max_value=1000000.0,
    value=float(st.session_state.budget),
    step=100.0
)
st.session_state.budget = budget

st.divider()
st.subheader("➕ Add a shopping item")

with st.form("add_item", clear_on_submit=True):
    name = st.text_input("Item name", placeholder="e.g. Milk")
    price = st.number_input(
        "Price per item (₹)", min_value=0.0,
        value=50.0, step=10.0
    )
    quantity = st.number_input(
        "Quantity", min_value=1, max_value=100,
        value=1, step=1
    )
    category = st.selectbox(
        "Category",
        ["Groceries", "Snacks", "Personal Care",
         "Stationery", "Other"]
    )
    essential = st.checkbox("Essential item", value=True)
    submitted = st.form_submit_button(
        "Add to my cart", type="primary",
        use_container_width=True
    )

    if submitted:
        if name.strip() and price > 0:
            st.session_state.items.append({
                "name": name.strip(),
                "price": price,
                "quantity": quantity,
                "category": category,
                "essential": essential
            })
            st.rerun()
        else:
            st.warning("Enter an item name and a price above zero.")

items = st.session_state.items
total = sum(i["price"] * i["quantity"] for i in items)
remaining = budget - total
essential_total = sum(
    i["price"] * i["quantity"]
    for i in items if i["essential"]
)

st.divider()
st.subheader("📊 Your budget dashboard")

c1, c2 = st.columns(2)
c1.metric("Shopping total", f"₹{total:,.2f}")
c2.metric("Budget remaining", f"₹{remaining:,.2f}")

st.caption(f"Essential items: ₹{essential_total:,.2f}")
st.progress(min(total / budget, 1.0))

if remaining < 0:
    st.error(f"Over budget by ₹{abs(remaining):,.2f}! Review your cart.")
elif remaining == 0:
    st.warning("Your budget is fully used.")
else:
    st.success(f"You have ₹{remaining:,.2f} left to spend.")

st.divider()
st.subheader("🧺 My shopping list")

if not items:
    st.info("Your cart is empty. Add your first item above!")
else:
    for index, item in enumerate(items):
        col1, col2 = st.columns([3, 1])
        with col1:
            label = "Essential" if item["essential"] else "Optional"
            st.write(f"**{item['name']}**")
            st.caption(
                f"{item['category']} · {label} · "
                f"{item['quantity']} × ₹{item['price']:.2f}"
            )
            st.write(
                f"Item total: ₹{item['price'] * item['quantity']:.2f}"
            )
        with col2:
            if st.button("🗑️", key=f"remove_{index}"):
                st.session_state.items.pop(index)
                st.rerun()
        st.divider()

    if st.button("Clear entire cart"):
        st.session_state.items = []
        st.rerun()

st.subheader("💡 Smart saving tips")

optional_items = [
    i for i in items if not i["essential"]
]

if remaining < 0:
    st.write(
        "Your list exceeds your budget. Review optional purchases "
        "and consider lower-cost alternatives."
    )
elif optional_items:
    optional_total = sum(
        i["price"] * i["quantity"] for i in optional_items
    )
    st.write(
        f"You have ₹{optional_total:.2f} in optional purchases. "
        "Review them if you want to save more."
    )
else:
    st.write(
        "Mark non-essential items as optional to explore "
        "more ways to save."
    )

st.caption(
    "SmartCart AI | A student-focused budgeting prototype"
)
