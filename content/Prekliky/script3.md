
```python
import os
from docx import Document

def txt_to_docx(input_folder):
    if not os.path.exists(input_folder):
        print(f"Folder does not exist: {input_folder}")
        return

    output_folder = os.path.join(input_folder, "docx_output")
    os.makedirs(output_folder, exist_ok=True)

    txt_files = [f for f in os.listdir(input_folder) if f.lower().endswith(".txt")]

    if not txt_files:
        print("No .txt files found in the folder.")
        return

    for txt_file in txt_files:
        txt_path = os.path.join(input_folder, txt_file)
        try:
            with open(txt_path, "r", encoding="utf-8") as f:
                content = f.read()
        except Exception as e:
            print(f"Error reading {txt_file}: {e}")
            continue

        doc = Document()
        for line in content.splitlines():
            doc.add_paragraph(line)

        docx_filename = os.path.splitext(txt_file)[0] + ".docx"
        docx_path = os.path.join(output_folder, docx_filename)

        try:
            doc.save(docx_path)
            print(f"Converted: {txt_file} -> {docx_filename}")
        except Exception as e:
            print(f"Error saving {docx_filename}: {e}")

    print(f"All done! DOCX files saved in: {output_folder}")

if __name__ == "__main__":
    input_folder = r"C:\Users\peter\OneDrive\Desktop\Vysualist - Copy (2)\Visualyst Starter\Brigadka\cleaned_texts\Agenti"
    txt_to_docx(input_folder)
```