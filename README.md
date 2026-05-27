import streamlit as st
from pathlib import Path

st.set_page_config(page_title="File Manager", page_icon="📁")

st.title("📁 File Manager")
st.markdown("---")

operation = st.selectbox(
    "Operation select karo",
    ["-- Select --", "📄 Create File", "📖 Read File", "✏️ Update File", "🗑️ Delete File"]
)

st.markdown("---")

if operation == "📄 Create File":
    st.subheader("Create a New File")
    name = st.text_input("File name likho (e.g. hello.txt)")
    data = st.text_area("Content likho")
    if st.button("Create File", type="primary"):
        if not name:
            st.warning("File name daalo.")
        else:
            path = Path(name)
            if path.exists():
                st.error("❌ File already exists.")
            else:
                try:
                    with open(path, "w") as f:
                        f.write(data)
                    st.success(f"✅ File **{name}** create ho gayi!")
                    
                except Exception as e:
                    st.error(f"Error: {e}")

elif operation == "📖 Read File":
    st.subheader("Read a File")
    name = st.text_input("File name likho")
    if st.button("Read File", type="primary"):
        if not name:
            st.warning("File name daalo.")
        else:
            path = Path(name)
            if not path.exists():
                st.error("❌ File exist nahi karti.")
            else:
                try:
                    content = path.read_text()
                    st.success("✅ File read ho gayi!")
                    st.text_area("File Content", value=content, height=200, disabled=True)
                except Exception as e:
                    st.error(f"Error: {e}")

elif operation == "✏️ Update File":
    st.subheader("Update a File")
    name = st.text_input("File name likho")
    if name:
        path = Path(name)
        if not path.exists():
            st.error("❌ File exist nahi karti.")
        else:
            action = st.radio("Kya karna hai?", ["Rename", "Append Content", "Overwrite Content"])
            if action == "Rename":
                new_name = st.text_input("Naya file name")
                if st.button("Rename", type="primary"):
                    if not new_name:
                        st.warning("Naya naam daalo.")
                    elif Path(new_name).exists():
                        st.error("❌ Us naam ki file pehle se hai.")
                    else:
                        try:
                            path.rename(Path(new_name))
                            st.success(f"✅ Rename ho gaya → **{new_name}**")
                        except Exception as e:
                            st.error(f"Error: {e}")
            elif action == "Append Content":
                append_data = st.text_area("Append karne wala content")
                if st.button("Append", type="primary"):
                    try:
                        with open(path, "a") as f:
                            f.write("\n" + append_data)
                        st.success("✅ Content append ho gaya!")
                    except Exception as e:
                        st.error(f"Error: {e}")
            elif action == "Overwrite Content":
                overwrite_data = st.text_area("Naya content (purana replace ho jayega)")
                if st.button("Overwrite", type="primary"):
                    try:
                        path.write_text(overwrite_data)
                        st.success("✅ File overwrite ho gayi!")
                    except Exception as e:
                        st.error(f"Error: {e}")

elif operation == "🗑️ Delete File":
    st.subheader("Delete a File")
    name = st.text_input("File name likho")
    if name:
        path = Path(name)
        if not path.exists():
            st.error("❌ File exist nahi karti.")
        else:
            st.warning(f"⚠️ **{name}** delete hoga — yeh undo nahi ho sakta!")
            if st.button("🗑️ Delete File", type="primary"):
                try:
                    path.unlink()
                    st.success(f"✅ File **{name}** delete ho gayi!")
                except Exception as e:
                    st.error(f"Error: {e}")
