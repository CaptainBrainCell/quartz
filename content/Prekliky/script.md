```python
import re
from pathlib import Path

#  Vault cesta
VAULT_PATH = Path(r"C:\Users\peter\OneDrive\Desktop\Vysualist\Visualyst Starter\Brigadka")
OUTPUT_PATH = VAULT_PATH / "cleaned_texts"

#  Vytvor výstupný priečinok
OUTPUT_PATH.mkdir(parents=True, exist_ok=True)

def clean_markdown(content):
    content = re.sub(r'!\[\[.*?\]\]', '', content)  # odstráni obrázky ![[img.png]]
    content = re.sub(r'\[\[.*?\]\]', '', content)   # odstráni interné odkazy [[note]]
    content = re.sub(r'\[([^\]]+)\]\([^)]+\)', r'\1', content)  # odkazy [text](url)
    content = re.sub(r'[*_~`>#-]', '', content)     # markdown znaky
    content = re.sub(r'\n{2,}', '\n\n', content)    # zredukuje viac prázdnych riadkov
    return content.strip()

def process_all_notes():
    md_files = list(VAULT_PATH.rglob("*.md"))
    if not md_files:
        print("❌ Nenašli sa žiadne .md súbory.")
        return

    for md_file in md_files:
        rel_path = md_file.relative_to(VAULT_PATH)
        output_file = OUTPUT_PATH / rel_path.with_suffix(".txt")
        output_file.parent.mkdir(parents=True, exist_ok=True)

        with open(md_file, encoding="utf-8") as f:
            raw = f.read()
        cleaned = clean_markdown(raw)

        with open(output_file, "w", encoding="utf-8") as f:
            f.write(cleaned)
        print(f" Vyčistené: {rel_path}")

if __name__ == "__main__":
    process_all_notes()
    print("\n HOTOVO! Vyčistené poznámky sú v priečinku 'cleaned_texts'")
```