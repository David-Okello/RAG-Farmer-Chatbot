# RAG Chatbot for Farmers

> An AI-driven, retrieval-augmented chatbot that taps into reports on soil health and water management to answer farmers’ questions in real time.

---

## 🚀 Key Features

- **Report Generation**  
  - Run `create_reports.py` to generate `.txt` reports (e.g. `soil_health_ea.txt`, `water_management.txt`) 
- **TF-IDF Prototype**  
  - Quick demo in `main.py` using Scikit-learn TF-IDF + KNN for passage retrieval.
- **Production-Ready Pipeline**  
  - `improved.py` leverages SentenceTransformers embeddings + FAISS for fast, semantic retrieval over large document sets.
- **Chunking Strategy**  
  - Automatically splits long reports into overlapping 500-token chunks for context-rich prompts.
- **Plug-and-Play LLM Support**  
  - Easily swap in your model of choice (OpenAI, Hugging Face, etc.) by adjusting a single config line.

---

## 📂 Repository Structure
    ├── reports/ # Generated text files

    │ ├── soil_health_ea.txt

    │ └── water_management.txt

    ├── create_reports.py # Script to build reports

    ├── main.py # TF-IDF + KNN retrieval prototype

    ├── improved.py # Semantic RAG with SentenceTransformers + FAISS

    ├── requirements.txt # Python dependencies

    └── README.md # ← You are here


---

## 🛠️ Getting Started

1. **Clone & Activate Environment**  
   ```bash
   git clone https://github.com/David-Okello/RAG-Farmer-Chatbot
   cd RAG-Farmer-Chatbot
   python -m venv .venv
   source .venv/bin/activate   # (or .\.venv\Scripts\Activate.ps1 on Windows)
    ```
2. **Install Dependencies**
    - pip install --upgrade pip
    - pip install -r requirements.txt

## 💡 Usage

1. Generate Reports
    ```bash
    python create_reports.py
    ```
    Creates reports/soil_health_ea.txt and reports/water_management.txt.

2. Run TF-IDF Prototype
    ```bash
    python main.py
    ```
    Answers a sample soil-health query using the TF-IDF + KNN approach.

3. Run Production RAG
    ```bash
    python improved.py \
    --docs_folder reports \
    --model_name all-MiniLM-L6-v2 \
    --top_k 5
    ```
    Splits each report into chunks

    Builds/loads a FAISS index

    Retrieves top-K relevant chunks

    Sends consolidated prompt to your chosen LLM

## 🏗️ Architecture & Design
- Chunking: Overlap of 100 tokens to preserve context boundaries.

- Embedding: Dense vector generation via SentenceTransformers.

- Indexing: FAISS index for sub-millisecond semantic search.

- Prompting: Dynamically concatenates retrieved chunks before LLM call.


## 🤝 Contributing
1. Fork this repo

2. Create a feature branch

3. Commit and push your changes

4. Open a Pull Request

Please update this README when adding new features or scripts.

## 📜 License
Distributed under the MIT License. See LICENSE for details.


