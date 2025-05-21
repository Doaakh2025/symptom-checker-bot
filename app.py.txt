import streamlit as st
import requests

st.set_page_config(page_title="Smart Medical Bot", layout="centered")
st.title("🤖 Smart Medical Bot")
st.markdown("Please enter your symptoms, and the bot will provide an initial medical suggestion.")

user_input = st.text_input("✍️ Describe your symptoms:")

if st.button("Submit"):
    if user_input.strip() == "":
        st.warning("⚠️ Please enter your symptoms before submitting.")
    else:
        url = "https://udify.app/chat/BXE7UHClh3Liaf7T"
        headers = {
            "Authorization": "Bearer app-MvMNfrvLnaD8mR9eKbP1Q4I1",
            "Content-Type": "application/json"
        }
        payload = {
            "inputs": user_input,
            "response_mode": "blocking"
        }

        with st.spinner("⏳ Please wait..."):
            try:
                response = requests.post(url, json=payload, headers=headers)
                response.raise_for_status()
                result = response.json()
                reply = result.get("answer") or result.get("response") or result.get("text") or "⚠️ No valid answer received."
                st.success("🩺 Bot's Response:")
                st.write(reply)
            except requests.exceptions.RequestException as e:
                st.error("🚨 Request failed:")
                st.code(str(e))
