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
   source .venv/bin/activate   # (or .\.venv\Scripts\Activate on Windows)
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

### improved.py
An improved version of the RAG prototype.
Think of main.py as RAG version 1 and improved.py as RAG version 1.1

SentenceTransformer gives you dense semantic embeddings (vs. TF-IDF’s keyword overlap).
FAISS is the industry standard for fast, approximate nearest-neighbor retrieval at scale.

a) Document Loading & Chunking
- Why chunk? LLMs have context-window limits (they get dumb😅). Breaking large reports into overlapping 500-token snippets preserves continuity at boundaries.

- Metadata (e.g. source=file.name) lets you filter by region, date, or document type at retrieval time.

b) Embedding & Indexing
- “all-MiniLM-L6-v2” balances speed vs. semantic power.

- L2 normalization + Inner-Product is mathematically equivalent to cosine similarity, which works well for nearest-neighbor retrieval.

- IndexFlatIP is the simplest FAISS index; for millions of docs you’d swap in an IVF or HNSW variant.

c) Retrieval + Prompt Composition
1. Embed the user query into the same semantic space.

2. Search the FAISS index for the top-k closest chunks.

3. Optional metadata filtering ensures, for example, only “East Africa” docs are returned.

4. Build a system + context prompt:

    - A clear “You are …” system instruction locks the chatbot’s persona.

    - Numbered contexts let you—and the LLM—trace back which passages informed the answer.

    - Appending User: … Bot: sets the stage for an LLM call (e.g. openai.ChatCompletion.create(prompt=prompt)).

High-Level Takeaways
1. Modularity: each function has a single responsibility (chunking, embedding, indexing, retrieval, prompting).

2. Scalability path:

    - Swap TF-IDF → dense embeddings for better semantic recall.

    - Scale FAISS → distributed IVF/HNSW for millions of docs.

    - Add caching of embeddings & prompts for low-latency.

3. Explainability: humans can inspect “which snippet #2 came from file X” to audit and debias the system.

4. Security: RAG prototypes should run over sanitized, internally hosted document stores behind VPCs to avoid exposing sensitive reports.

### main.py
Sample RAG prototype: vectorize simple soil-health docs, retrieve top-3, build prompt.

1. Sample “soil-health” documents (simulating indexed World Bank reports)
2. Build TF-IDF embeddings & NearestNeighbors index
3. Embed query
4. Retrieve top-3 docs
5. Compose prompt
6. Show results

output generated: 
```
(.venv) C:\Users\User\Desktop\WorldBankFarmerChat>python main.py
=== Retrieved Passages ===
1. Regular soil testing reveals nitrogen, phosphorus, and potassium deficiencies.
2. Compost application increases microbial activity and soil structure quality.
3. Cover cropping reduces erosion and boosts soil biodiversity over time.

=== Generated Prompt ===
You are an agricultural expert chatbot. Use the following contexts to answer:

1. Regular soil testing reveals nitrogen, phosphorus, and potassium deficiencies.

2. Compost application increases microbial activity and soil structure quality.

3. Cover cropping reduces erosion and boosts soil biodiversity over time.

User: What soil health indicators should a farmer monitor?
Bot:

```

## 🤝 Contributing
1. Fork this repo

2. Create a feature branch

3. Commit and push your changes

4. Open a Pull Request

Please update this README when adding new features or scripts.

## 📜 License
Distributed under the MIT License. See LICENSE for details.

## 📬 Contact
Website: https://david-okello.webflow.io/

LinkedIn: https://www.linkedin.com/in/david-okello-3599b51a0/

Email: okellodavid002@gmail.com 
