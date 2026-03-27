# Telemedicine-app-users-
import streamlit as st
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

st.title("📱 Telemedicine App User Growth & Load Analysis")

# Sidebar Inputs (Sensitivity Analysis)
st.sidebar.header("Input Parameters")

initial_users = st.sidebar.slider("Initial Users", 100, 10000, 500)
growth_rate = st.sidebar.slider("Growth Rate (%)", 1, 50, 10)
days = st.sidebar.slider("Number of Days", 10, 100, 30)
appointments_per_user = st.sidebar.slider("Appointments per User", 0.1, 2.0, 0.5)
max_capacity = st.sidebar.slider("Max Daily Capacity", 100, 10000, 2000)

# Growth Model (Exponential Growth)
time = np.arange(days)
users = initial_users * (1 + growth_rate / 100) ** time

# Daily Appointments
appointments = users * appointments_per_user

# Peak Load Detection
peak_load = np.max(appointments)
peak_day = np.argmax(appointments)

# Dataframe
df = pd.DataFrame({
    "Day": time,
    "Users": users,
    "Appointments": appointments
})

# Display Data
st.subheader("📊 Data Preview")
st.write(df.head())

# Plot Graph
st.subheader("📈 Growth & Appointments Graph")

plt.figure()
plt.plot(time, users, label="Active Users")
plt.plot(time, appointments, label="Daily Appointments")
plt.axhline(y=max_capacity, linestyle='--', label="Max Capacity")

plt.xlabel("Days")
plt.ylabel("Count")
plt.legend()

st.pyplot(plt)

# Peak Load Info
st.subheader("⚠️ Peak Load Analysis")
st.write(f"Peak Load: {int(peak_load)} appointments")
st.write(f"Peak occurs on Day: {peak_day}")

# Capacity Warning
if peak_load > max_capacity:
    st.error("⚠️ Capacity Exceeded! System may overload.")
else:
    st.success("✅ Within Capacity Limits")

# Marketing Effect Simulation
st.subheader("📢 Marketing Impact Simulation")

marketing_boost = st.slider("Marketing Boost (%)", 0, 100, 20)

boosted_users = users * (1 + marketing_boost / 100)
boosted_appointments = boosted_users * appointments_per_user

plt.figure()
plt.plot(time, boosted_users, label="Users After Marketing")
plt.plot(time, boosted_appointments, label="Appointments After Marketing")

plt.xlabel("Days")
plt.ylabel("Count")
plt.legend()

st.pyplot(plt)
