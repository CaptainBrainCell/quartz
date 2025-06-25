
```python
import os
import re

def load_txt_files_from_folder(folder_path):
    return [
        os.path.join(folder_path, f)
        for f in os.listdir(folder_path)
        if f.endswith(".txt")
    ]

def load_document(file_path):
    with open(file_path, 'r', encoding='utf-8') as file:
        return file.read()

def extract_agents(text):
    pattern = r"# (.*?)\n(.*?)(?=\n# |\Z)"  # Od # názvu po ďalší # alebo koniec textu
    matches = re.findall(pattern, text, re.DOTALL)
    agents = []

    for name, body in matches:
        agent = {
            "name": name.strip(),
            "popis": "",
            "postup": "",
            "poznamky": ""
        }

        popis = re.search(r"POPIS\s*(.*?)(?=\n[A-Z]{4,}|$)", body, re.DOTALL)
        postup = re.search(r"POSTUP\s*(.*?)(?=\n[A-Z]{4,}|$)", body, re.DOTALL)
        pozn = re.search(r"POZNÁMKA\s*(.*)", body, re.DOTALL)

        if popis: agent["popis"] = popis.group(1).strip()
        if postup: agent["postup"] = postup.group(1).strip()
        if pozn: agent["poznamky"] = pozn.group(1).strip()

        agents.append(agent)
    return agents

def save_agent_to_txt(agent, output_folder):
    safe_name = re.sub(r'[\\/*?:"<>|]', "", agent["name"])
    filename = os.path.join(output_folder, f"{safe_name}.txt")
    
    with open(filename, 'w', encoding='utf-8') as f:
        f.write(f"# {agent['name']}\n\n")
        if agent["popis"]:
            f.write("POPIS:\n" + agent["popis"] + "\n\n")
        if agent["postup"]:
            f.write("POSTUP:\n" + agent["postup"] + "\n\n")
        if agent["poznamky"]:
            f.write("POZNÁMKA:\n" + agent["poznamky"] + "\n\n")
    print(f" Uložené: {filename}")

def main(folder_path):
    output_folder = os.path.join(folder_path, "output_agents")
    os.makedirs(output_folder, exist_ok=True)

    txt_files = load_txt_files_from_folder(folder_path)
    if not txt_files:
        print("❌ Žiadne .txt súbory neboli nájdené.")
        return

    total_saved = 0

    for file_path in txt_files:
        text = load_document(file_path)
        agents = extract_agents(text)
        for agent in agents:
            save_agent_to_txt(agent, output_folder)
            total_saved += 1

    print(f"\n Hotovo! Spolu uložených agentov: {total_saved}")
    print(f" Výstupný priečinok: {output_folder}")

if __name__ == "__main__":
    folder_path = r"C:\Users\peter\OneDrive\Desktop\Vysualist - Copy (2)\Visualyst Starter\Brigadka\cleaned_texts\Agenti"
    main(folder_path)
```