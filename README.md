A **vector database** stores and queries high-dimensional vectors, which are numerical representations of data like text, images, or audio, often used in AI applications. It enables **similarity searches** using metrics like cosine similarity or Euclidean distance.

### Key Uses:
- **Search Engines**: Semantic search based on meaning, not keywords.
- **Recommendations**: Suggest items by vector similarity.
- **NLP**: Power chatbots, document retrieval, and summarization.
- **Image/Audio Processing**: Reverse search or grouping similar media.

### Popular Tools:
- **Pinecone**, **Weaviate**, **Milvus**, **Chroma** (focuses on embedding retrieval), **Faiss**, and **Vespa**.

These databases are faster and scalable for AI-driven tasks compared to traditional databases. **Chroma**, like Pinecone, specializes in embedding management and retrieval, making it ideal for modern AI applications.
### **What is Chroma?**
- **Chroma**: An open-source AI-native vector database for storing and querying embeddings (numerical representations of data like text or images). It's fast, developer-friendly, and doesn't need credentials for basic use.

---

### **Setup**
1. **Install Chroma Integration**:
   ```bash
   pip install -qU "langchain-chroma>=0.1.2"
   ```
2. **Optional Tracing** (track model calls):
   ```python
   os.environ["LANGSMITH_API_KEY"] = "your_key"
   os.environ["LANGSMITH_TRACING"] = "true"
   ```

---

### **Initialization**
1. **Basic Initialization**:
   - Save data locally with `persist_directory`:
     ```python
     vector_store = Chroma(
         collection_name="example_collection",
         embedding_function=embeddings,
         persist_directory="./chroma_langchain_db",
     )
     ```

2. **From Chroma Client**:
   - Use `chromadb` for better database access:
     ```python
     persistent_client = chromadb.PersistentClient()
     collection = persistent_client.get_or_create_collection("collection_name")
     ```

---

### **Adding Items**
- Add documents with metadata and unique IDs:
   ```python
   document = Document(page_content="Your text here", metadata={"source": "tweet"})
   vector_store.add_documents(documents=[document])
   ```

---

### **Updating Items**
- Update an existing document:
   ```python
   vector_store.update_document(document_id="your_id", document=updated_document)
   ```
- Update multiple documents at once:
   ```python
   vector_store.update_documents(ids=["id1", "id2"], documents=[doc1, doc2])
   ```

---

### **Deleting Items**
- Remove items from the vector store:
   ```python
   vector_store.delete(ids=["id_to_delete"])
   ```

---

### **Querying**
1. **Similarity Search**:
   - Find documents similar to your query:
     ```python
     results = vector_store.similarity_search("Your query here", k=2)
     ```
   - Example output:
     ```
     * Building an exciting new project! [{'source': 'tweet'}]
     ```

2. **Similarity Search with Scores**:
   - Includes how closely results match:
     ```python
     results = vector_store.similarity_search_with_score("Your query", k=1)
     ```
   - Example output:
     ```
     * [SIM=1.726390] The stock market is down. [{'source': 'news'}]
     ```

3. **Search by Vector**:
   - Search using custom embeddings:
     ```python
     embedding = embeddings.embed_query("I love green eggs!")
     results = vector_store.similarity_search_by_vector(embedding, k=1)
     ```

---

### **Retriever**
- Turn vector store into a retriever for chaining:
   ```python
   retriever = vector_store.as_retriever(search_type="mmr", search_kwargs={"k": 1})
   retriever.invoke("Query text", filter={"source": "news"})
   ```

---

### **Key Concepts**
1. **Embeddings**: Numerical representation of text or data for efficient search and matching.
2. **Vector Store**: A database to store embeddings for searching and querying.
3. **Similarity Search**: Finds documents related to your input query.
4. **Persistence**: Save and reuse data locally (`persist_directory`).

## **The Workflow of Chroma in 5 Simple Steps**

### **Step 1: Set Up the Magical Library**
Before you can store anything, you need to set up the library. This is where all your books (data) will be kept. 

- **What to do?**
  1. Create a place (called `persist_directory`) where the library will save your books.
  2. Initialize the library using Chroma.

```python
from langchain.vectorstores import Chroma

# Set up the magical library
vector_store = Chroma(
    collection_name="my_collection",  # Give your library a name
    embedding_function=embeddings,   # Helper to understand your books
    persist_directory="./chroma_library",  # Where your library lives
)
```

---

### **Step 2: Bring a Book to the Library**
You can bring any book (text file or data) to your library. Let’s say you have a file called `doctumt.txt`.

- **What to do?**
  1. Upload the file to your computer or Colab.
  2. Read the content of the file.

```python
# Upload the file in Colab
from google.colab import files
uploaded = files.upload()

# Open and read the file
with open("doctumt.txt", "r") as file:
    content = file.read()  # This is the story inside your book
```

---

### **Step 3: Store the Book in the Library**
Once you’ve read the book, you can store it in the library. Each book gets:
- A unique ID (like a label to find it later).
- Some extra info (like metadata about the book, e.g., "Where is this book from?").

- **What to do?**
  Add the book’s content to your magical library.

```python
# Add the content to the library
vector_store.add_texts(
    texts=[content],  # The story you’re adding
    metadatas=[{"source": "doctumt.txt"}],  # Info about the book
    ids=["book1"]  # A name for this book
)
```

---

### **Step 4: Save the Library**
To make sure your books don’t disappear, you need to save the library. This is like locking it safely.

- **What to do?**
  Save the library so you can reopen it later.

```python
# Save the library
vector_store.persist()
```

---

### **Step 5: Open the Library Later**
When you come back tomorrow (or next week), you can open your magical library and find all your books.

- **What to do?**
  1. Load the library from the saved location.
  2. Search or read the books you stored.

```python
# Open the library
vector_store = Chroma(
    collection_name="my_collection",
    embedding_function=embeddings,
    persist_directory="./chroma_library",
)

# Now you can find your books!
```

---

## **Visual Workflow**
1. **Set Up the Library** → Create a magical storage place.
2. **Bring a Book** → Upload a file or some data.
3. **Store the Book** → Save the file’s content in the library.
4. **Save the Library** → Make sure the library stays safe.
5. **Open the Library Later** → Access it anytime to find your stored content.
   https://python.langchain.com/docs/integrations/vectorstores/chroma/

