# Context-Aware Chatbot for Medical Domain

This project implements a Context-Aware Chatbot designed specifically for the medical domain. The chatbot can process a variety of document types (PDF, DOCX, TXT, CSV, XLSX) and provide relevant answers to user queries based on the context extracted from these documents. The chatbot utilizes FastAPI for API endpoints, Langchain for document processing, and Ollama for language model-based question answering.

The primary goal of this project, developed for the Bajaj Finserv Hackathon, is to create an AI-powered assistant that can assist users in navigating medical data, such as patient records, medical guidelines, research papers, and other medical documents.

# Features

Question Answering: Users can ask questions, and the chatbot will provide relevant answers based on the context extracted from uploaded medical documents.
Document Ingestion: Upload various document types (PDF, DOCX, TXT, CSV, XLSX) containing medical data, and the chatbot will process and store the content for further querying.
Context Awareness: The chatbot maintains session-based chat history and integrates it with document context to provide more accurate and relevant responses.
Error Handling: The chatbot gracefully handles errors in case of missing documents or failed text extraction.

# Requirements

Python Packages
fastapi - Web framework for building APIs.
pydantic - Data validation and settings management.
pandas - Data manipulation and analysis.
langchain-community - A library for creating language models with tools and integrations.
Chroma - Vector store for document storage and retrieval.
Ollama - Open-source large language model (LLM) for text generation.
docx - Library for reading DOCX files.
PyPDF2 - Library for reading PDF files.
openai - For interacting with OpenAI API, if needed.
tempfile - For handling temporary file storage.
numpy - For matrix operations (if needed).


# Endpoints
1. Chat with the Bot
This endpoint allows users to interact with the chatbot. It will respond to questions based on the context extracted from the documents uploaded by the user.

URL: /chat/
Method: POST
Headers:
x-user-id: User ID (string).
x-session-id: Session ID (string).
Body:

{
    "query": "Your question here"
}
Response:


{
    "response": "Your answer here"
}

2. Ingest Document
This endpoint allows users to upload files (PDF, DOCX, TXT, CSV, XLSX), and the chatbot will process and store the contents for future querying.

URL: /ingest_file/

Method: POST

Body:

file: The document file to be ingested (PDF, DOCX, TXT, CSV, XLSX).
Response:

json

{
    "status": "File ingested successfully"
}

3.Error Handling
If the file cannot be processed or text cannot be extracted, the response will be:



{
    "status": "Failed to extract text"
}
If there is any error in the system, the response will be:



{
    "status": "Error: <error_message>"
}
System Architecture
![image](https://github.com/user-attachments/assets/3a782d8e-65d1-4da7-b4c8-21b9f3a55c0e)



# How it Works
Text Extraction: When a document is uploaded, the appropriate extraction function is called based on the file type (PDF, DOCX, TXT, CSV, XLSX).
Text Splitting: The extracted text is then split into smaller chunks to maintain relevance and prevent overload when storing into the vector database.
Embedding & Vector Storage: Text chunks are embedded using the SentenceTransformerEmbeddings model and stored in the Chroma vector store.
Question Answering: When a user asks a question, the chatbot retrieves the most relevant chunks from the vector store, formats them with chat history, and generates an answer using the LLM.
Session Management: The chatbot stores the chat history for each user and session to provide context for subsequent questions.

# Deployment
.

1. Set Up Uvicorn with Nginx
Step 1: Install Dependencies
You’ll need Uvicorn and Gunicorn to run your FastAPI app:

pip install uvicorn gunicorn

Step 2: Start the FastAPI Application with Uvicorn
To run the FastAPI app with Uvicorn, use the following command:

uvicorn main:app --host 0.0.0.0 --port 8000 --reload

Step 3: Set Up Gunicorn for Production
For production, it's recommended to use Gunicorn with Uvicorn workers:


gunicorn -w 4 -k uvicorn.workers.UvicornWorker main:app --bind 0.0.0.0:8000

All Set the Api will return the response 

![image](https://github.com/user-attachments/assets/850c8d12-ac37-4c0e-a7b1-4ebbbc9588d7)
![image](https://github.com/user-attachments/assets/39433d74-6401-467a-9553-1b21dacea601)
![image](https://github.com/user-attachments/assets/71534e20-0249-485b-a4e8-c570abd18286)


