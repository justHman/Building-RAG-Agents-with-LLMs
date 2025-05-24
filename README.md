# Building-RAG-Agents-with-LLMs
Step 1: Tìm bài báo mới nhất trong vòng 1 tháng!

![image](https://github.com/user-attachments/assets/6b236d2a-d686-430a-8726-d52916b5bb17)
![image](https://github.com/user-attachments/assets/dc79a7f8-c4ce-404e-81c6-ebc3d01d2041)

Step 2: Chạy notebook 7 (07_vectorstores.ipynb)

![image](https://github.com/user-attachments/assets/a117e902-937e-4211-b414-a3dd63f048ff)

Ở task 1: thêm ID của bài báo đã tìm ở step 1 vào cell trong hình
Ví dụ: [1] arXiv:2505.00040 [pdf, html, other] -> ArxivLoader(query="2505.00040").load(),

Sau đó chạy các cell còn lại để tạo ra file docstore_index.tgz

Step 3: Vào notebook 9 tìm phần hình dưới

![image](https://github.com/user-attachments/assets/08ad2191-076b-457a-aa36-cca008c645c4)

Thay đổi code trong cell bằng code bên dưới và chạy cell đó

#===============================================================================================================
%%writefile server_app.py
# https://python.langchain.com/docs/langserve#server
from fastapi import FastAPI
from langchain_nvidia_ai_endpoints import ChatNVIDIA, NVIDIAEmbeddings
from langserve import add_routes

## May be useful later
from langchain_core.output_parsers import StrOutputParser
from langchain_core.prompts import ChatPromptTemplate, PromptTemplate
# from langchain_core.prompt_values import ChatPromptValue # Not explicitly used here
from langchain_core.runnables import RunnableLambda, RunnableBranch, RunnablePassthrough
# from langchain_core.runnables.passthrough import RunnableAssign # Not explicitly used in final chains here
# from langchain_community.document_transformers import LongContextReorder # Frontend handles this
from functools import partial
from operator import itemgetter

from langchain_community.vectorstores import FAISS
import os # For checking/extracting tgz

print("Initializing server_app.py...")

# --- Configuration ---
# TODO: Make sure to pick your LLM and do your prompt engineering as necessary for the final assessment
# Using default models from the course for consistency with assessment expectations
EMBEDDER_MODEL_NAME = "nvidia/nv-embed-v1" # Or your chosen embedder
CHAT_MODEL_NAME = "meta/llama3-8b-instruct"  # Or your chosen chat model for basic chat
GENERATOR_MODEL_NAME = "meta/llama3-8b-instruct" # Or your chosen model for RAG generation

# Initialize Embedder and LLMs
try:
    embedder = NVIDIAEmbeddings(model=EMBEDDER_MODEL_NAME, truncate="END")
    print(f"Embedder '{EMBEDDER_MODEL_NAME}' initialized.")
except Exception as e:
    print(f"ERROR initializing embedder '{EMBEDDER_MODEL_NAME}': {e}")
    # Fallback or raise error
    raise

try:
    basic_chat_llm = ChatNVIDIA(model=CHAT_MODEL_NAME)
    print(f"Basic Chat LLM '{CHAT_MODEL_NAME}' initialized.")
except Exception as e:
    print(f"ERROR initializing Basic Chat LLM '{CHAT_MODEL_NAME}': {e}")
    raise

try:
    generator_llm = ChatNVIDIA(model=GENERATOR_MODEL_NAME) # Used by the /generator endpoint
    print(f"Generator LLM '{GENERATOR_MODEL_NAME}' initialized.")
except Exception as e:
    print(f"ERROR initializing Generator LLM '{GENERATOR_MODEL_NAME}': {e}")
    raise

# --- FastAPI App ---
app = FastAPI(
  title="LangChain Server for DLI RAG Course",
  version="1.0",
  description="API server for RAG agent components using Langchain Runnable interfaces.",
)

# --- Helper Function for docs_to_str (if needed by any chain directly, though frontend handles it for RAG) ---
# def docs2str(docs, title="Document"):
#     out_str = ""
#     for doc_obj in docs:
#         doc_name = getattr(doc_obj, 'metadata', {}).get('Title', title)
#         if doc_name: out_str += f"[Quote from {doc_name}] "
#         out_str += getattr(doc_obj, 'page_content', str(doc_obj)) + "\\n"
#     return out_str

# --- Load FAISS Vector Store ---
DOCSTORE_PATH = "docstore_index"
DOCSTORE_TGZ_PATH = "docstore_index.tgz"
docstore_loaded = None

if not os.path.exists(DOCSTORE_PATH):
    print(f"Directory '{DOCSTORE_PATH}' not found.")
    if os.path.exists(DOCSTORE_TGZ_PATH):
        print(f"Attempting to extract '{DOCSTORE_TGZ_PATH}'...")
        try:
            os.system(f"tar xzvf {DOCSTORE_TGZ_PATH}")
            if os.path.exists(DOCSTORE_PATH):
                print(f"Successfully extracted to '{DOCSTORE_PATH}'.")
            else:
                print(f"ERROR: Extraction failed, '{DOCSTORE_PATH}' still not found.")
        except Exception as e:
            print(f"ERROR during extraction: {e}")
    else:
        print(f"ERROR: '{DOCSTORE_TGZ_PATH}' not found. Cannot create vector store.")

if os.path.exists(DOCSTORE_PATH):
    try:
        print(f"Loading FAISS index from '{DOCSTORE_PATH}'...")
        docstore_loaded = FAISS.load_local(DOCSTORE_PATH, embedder, allow_dangerous_deserialization=True)
        print(f"FAISS index loaded successfully with {len(docstore_loaded.docstore._dict) if docstore_loaded.docstore else 0} documents.")
    except Exception as e:
        print(f"ERROR loading FAISS index from '{DOCSTORE_PATH}': {e}")
        print("Retriever endpoint might not function correctly.")
else:
    print(f"ERROR: Path '{DOCSTORE_PATH}' does not exist after attempting extraction. Vector store not loaded.")

if docstore_loaded is None:
    # Fallback: Create an empty or minimal FAISS store if loading fails, so the server can still start
    print("WARNING: docstore_loaded is None. Creating a fallback FAISS store.")
    docstore_loaded = FAISS.from_texts(["Error: Vector store index could not be loaded. Please check server logs and ensure docstore_index.tgz is present and valid."], embedder)

# --- Define Chains for Endpoints ---

# 1. Basic Chat Endpoint (already provided in template)
# Input: String (user message) or list of messages
# Output: AIMessage or String (LLM response)
# The frontend for "Basic" tab expects a simple LLM call.
# `basic_chat_llm` directly can be used.

# 2. Retriever Endpoint
# Input: String (user query)
# Output: List[Document] (list of Langchain Document objects)
# The frontend_server.py will take this list, apply LongContextReorder, and then docs2str.
retriever_chain = docstore_loaded.as_retriever()
print("Retriever chain defined.")

# 3. Generator Endpoint
# Input: Dict {'input': str (user_query), 'context': str (retrieved_context_as_string)}
# Output: String (final LLM answer for RAG)
generator_prompt_text = (
    "You are a helpful AI assistant. Based on the following retrieved context, "
    "answer the user's question. If the context doesn't provide enough information, "
    "say that you cannot answer based on the provided documents. "
    "Do not make up information. Be concise and directly answer the question.\n\n"
    "Retrieved Context:\n{context}\n\n"
    "User's Question: {input}\n\n"
    "Your Answer:"
)
generator_prompt = ChatPromptTemplate.from_template(generator_prompt_text)
generator_chain = generator_prompt | generator_llm | StrOutputParser()
print("Generator chain defined.")


# --- Add Routes to FastAPI App ---

# PRE-ASSESSMENT: Run as-is and see the basic chain in action
add_routes(
    app,
    basic_chat_llm, # This is a ChatNVIDIA instance, which is a Runnable
    path="/basic_chat",
    # config_keys=["configurable"], # Example, if you want to pass config
    # input_type=str, # LangServe can often infer this
    # output_type=str,
)
print("Route /basic_chat added.")

# ASSESSMENT TODO: Implement these components as appropriate
add_routes(
    app,
    generator_chain,
    path="/generator",
    # input_type={"input": str, "context": str}, # Explicitly define if needed
    # output_type=str,
)
print("Route /generator added.")

add_routes(
    app,
    retriever_chain,
    path="/retriever",
    # input_type=str,
    # output_type=list # List of Document objects
)
print("Route /retriever added.")

print("All routes added. Server setup complete.")

## Might be encountered if this were for a standalone python file...
if __name__ == "__main__":
    import uvicorn
    print("Starting Uvicorn server...")
    uvicorn.run(app, host="0.0.0.0", port=9012)
#===============================================================================================================

Chạy tiếp cell sau để tạo ra file server_app.py:

![image](https://github.com/user-attachments/assets/746a4ffb-0299-4e37-b575-af5da1d41cc4)

Step 4: Chạy cell dưới ảnh để tạo ra link: Gradio

![image](https://github.com/user-attachments/assets/a5a79cd4-be09-4034-b808-b009bac477b9)

Rồi bấm vào link và Evalue model
Sau khi chạy xong -> Bấm Acesstask như hình dưới

![image](https://github.com/user-attachments/assets/dca604a1-cd30-4bdf-9f67-c8e700194db8)






