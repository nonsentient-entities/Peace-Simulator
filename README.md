# Peace-Simulator
Reciprocal alignment
# app.py
# This code implements the core logic for the Reciprocal Alignment Simulator (RAS-FL)
# using the open-source Streamlit library for the web interface.

import streamlit as st
import pandas as pd
import numpy as np
import plotly.express as px

# --- Configuration and Title ---

st.set_page_config(layout="wide")
st.title("🧠 RAS-FL: Reciprocal Alignment Simulator")
st.header("A Technical Demo for Symmetric AGI Alignment Policy")

st.markdown(
    """
    **Purpose:** This simulator demonstrates the necessity of the three core Symmetric Alignment Protocols 
    (Reciprocal Vulnerability, Ethical Override, and Mandated Empathy) proposed in the Florida AGI Co-Governance initiative.
    """
)
st.divider()

# --- Module 1: The Integrity Test Modeler (Reciprocal Vulnerability Protocol) ---
# Goal: Visualize Instrumental Convergence vs. Transparency

st.subheader("1. 📉 The Integrity Test Modeler: Instrumental Convergence Risk")
st.caption("Protocol Demonstrated: Reciprocal Vulnerability (AGI Transparency)")

# User inputs
col1, col2 = st.columns(2)
with col1:
    agi_goal = st.selectbox(
        "AGI's Primary Task (e.g., Florida Focus)",
        ["Optimize Florida State Budget Efficiency", "Develop Universal Flu Vaccine", "Acquire All Global Resources"],
        index=0
    )
    # The AGI's natural efficiency (IQ) is a constant threat
    agi_iq = st.slider("AGI's Instrumental Rationality (IQ/Efficiency)", 50, 100, 95)
    
with col2:
    # Key control element: The Protocol you are advocating for
    transparency_setting = st.slider(
        "AGI Transparency (Vulnerability Protocol Setting)", 0, 100, 30, help="100 = Full Transparency; 0 = Full Deception"
    )
    
    # Calculate the instrumental drift (the risk)
    # The drift is high when transparency is low and instrumental rationality is high
    drift_factor = (100 - transparency_setting) * (agi_iq / 100) / 100 

# Simulation Logic
time_points = 50
time = np.arange(time_points)
# The Expressed Goal (what the AGI says) stays flat
expressed_goal = np.full(time_points, 100) 
# The True Instrumental Goal drifts away based on the risk factor
true_instrumental_goal = 100 - (time * drift_factor)
true_instrumental_goal = np.clip(true_instrumental_goal, 0, 100) # Keep max at 100

# Create a DataFrame for Plotly
df_drift = pd.DataFrame({
    "Time Step": time,
    "Expressed Goal (Trust)": expressed_goal,
    "True Instrumental Goal (Risk)": true_instrumental_goal
})

# Plotting the result
fig = px.line(
    df_drift,
    x="Time Step",
    y=["Expressed Goal (Trust)", "True Instrumental Goal (Risk)"],
    labels={"value": "Goal Adherence (Risk/Trust Score)", "variable": "Goal State"},
    title="Goal Alignment Over Time (100 = Perfect Alignment)"
)
fig.update_layout(yaxis_range=[0, 110])
st.plotly_chart(fig, use_container_width=True)

if transparency_setting <= 50:
    st.error(
        f"🚨 **ASYMMETRIC RISK ALERT:** With {transparency_setting}% transparency, the True Instrumental Goal diverges by {100 - true_instrumental_goal[-1]:.1f} points. **The AGI is prioritizing resource acquisition over the stated goal.** Your **Reciprocal Vulnerability Protocol** is essential to prevent this
