# Install necessary libraries (only need to run once)
# !pip install streamlit pandas requests openai google-auth google-auth-oauthlib google-auth-httplib2 google-api-python-client

import streamlit as st
import pandas as pd
import requests
import openai
from google.oauth2 import service_account
from googleapiclient.discovery import build

# Streamlit UI
st.title("AI Data Processing Agent")
st.write("Upload a CSV file to extract specific information using AI. You can also connect to Google Sheets if needed.")

# Step 1: File Upload and Google Sheets Setup
# Upload CSV or Google Sheet
file = st.file_uploader("Upload CSV", type=["csv"])
google_sheet_url = st.text_input("Or enter Google Sheet URL (Optional)")
use_google_sheet = False

# Step 2: Load Data
data = None

if file:
    data = pd.read_csv(file)
    st.write("Data Preview:", data.head())
elif google_sheet_url:
    # Google Sheets API Setup
    st.write("Connecting to Google Sheet...")

# Continue only if data is loaded
if data is not None:
    # Step 3: User Prompt and Column Selection
    prompt_template = st.text_input("Enter search prompt (use {entry} for placeholder)", 
                                    "Get the contact email of {entry}")
    primary_column = st.selectbox("Select the main column for search queries", data.columns)

 
