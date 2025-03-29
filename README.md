# Building-an-LLM-Powered-Booking-Analytics-and-QA-System

## Objective
Develop a system that processes hotel booking data, extracts insights, and enables retrieval-augmented question answering (RAG). The system provides analytical insights and answers user queries about the data.

## Key Features
- **Data Collection & Preprocessing:** Clean and structure the data.
- **Analytics & Reporting:** Generate various analytics such as revenue trends, cancellation rates, geographical distribution, and booking lead time.
- **RAG Implementation:** Use FAISS with an open-source LLM (Flan-T5) to answer booking-related questions.
- **API Development:** Provide REST API endpoints for analytics, question answering, history, and health checks.
- **Performance Evaluation:** Assess the system's accuracy and response time.

---

## Project Setup

### Environment
- **Google Colab:** Python 3 with T4 GPU runtime.
- **Database:** SQLite for storing clean data, chat history, and embeddings.

### Required Libraries
```
!pip install pandas matplotlib seaborn faiss-cpu sentence-transformers transformers ipython-sql fastapi uvicorn nest-asyncio pyngrok scikit-learn numpy
```

---

## Steps to Run the Project

### Step 1: Data Collection & Preprocessing
**Dataset:**
- We used a sample dataset (`hotel_bookings.csv`) loaded from Google Drive.
- Original data is cleaned to handle missing values and inconsistent formats.

**Preprocessing:**
- Cleaned columns such as `children`, `country`, `agent`, and `company`.
- Created derived features such as `total_nights` and `revenue`.

---

### Step 2: Analytics & Reporting
**Charts Generated:**
- Revenue trends over time
- Cancellation rate percentage
- Geographical distribution of bookings
- Booking lead time distribution

**Libraries Used:**
- `matplotlib`, `seaborn`, and `pandas` for visualizing data.

---

### Step 3: Retrieval-Augmented Question Answering (RAG)
**Embedding & Vectorization:**
- Vector embeddings created using `multi-qa-mpnet-base-dot-v1` from Sentence Transformers.
- FAISS is used to store and query embeddings.

**LLM Integration:**
- `google/flan-t5-large` for text generation and answering questions.

**Embedding Generation:**
- Text embeddings are generated from the cleaned data and stored as two `.npy` files:
  - `embedded_texts.npy`: Contains the text data embeddings.
  - `embeddings.npy`: Contains the numerical vector representations of the embedded text.
- Metadata and embeddings are stored in the SQLite database (`hotel_bookings.db`).

---

### Step 4: API Development
**API Framework:**
- FastAPI used for API endpoints.

**Endpoints Implemented:**
- `POST /analytics` → Returns analytics reports.
- `POST /ask` → Answers booking-related questions.
- `GET /history` → Retrieves query history.
- `DELETE /history/clear` → Clears query history.
- `GET /health` → Checks system health.

**History & Logging:**
- Chat history and responses stored in `hotel_bookings.db`.
- Logs include timestamps and query-response pairs.

**How to Run API using Ngrok in Google Colab:**

- Get Ngrok Auth Token:
  - Go to [ngrok.com](https://dashboard.ngrok.com/get-started/setup) and sign up/log in.
  - After logging in, go to the **Dashboard** and navigate to the **Setup & Installation** tab.
  - Copy your authtoken from the displayed command, which looks like:
  ```
  ngrok config add-authtoken <YOUR_NGROK_AUTHTOKEN>
  ```
- Set Ngrok Auth Token:
  - To configure the Ngrok authentication token:
  ```
  from pyngrok import ngrok
  ngrok.set_auth_token("<YOUR_NGROK_AUTHTOKEN>")
  ```
- Run Uvicorn with Ngrok:
  - To run Uvicorn and expose the API through Ngrok, use the following code:
  ```
  from fastapi import FastAPI
  import uvicorn
  
  app = FastAPI()
  
  # Establish a secure public URL using ngrok
  public_url = ngrok.connect(8000)
  print(f"Public URL: {public_url}")
  
  # Start Uvicorn server
  uvicorn.run(app, host="0.0.0.0", port=8000)
  ```

---

### Step 5: Performance Evaluation
**Accuracy**
- **Metric:** Cosine similarity between expected and generated answers.
- **Model Used:** `all-MiniLM-L6-v2` from Sentence Transformers.
- Evaluated using sample booking-related questions.

**Response Time**
- Measured using Python's time and requests libraries.

**Usage**
- Access API Endpoints
  - Analytics Endpoint:
  ```
  curl -X POST http://localhost:8000/analytics
  ```
  - Ask Endpoint:
  ```
  curl -X POST http://localhost:8000/ask -H "Content-Type: application/json" \
    -d '{"question": "Show me total revenue for July 2017."}'
  ```
  - Retrieve Chat History:
  ```
  curl -X GET http://localhost:8000/history
  ```
  - Health Check Endpoint:
  ```
  curl -X GET http://localhost:8000/health
  ```
  - Clear Chat History:
  ```
  curl -X DELETE http://localhost:8000/history/clear
  ```

---

## Conclusion
This project successfully demonstrates the integration of LLMs with vector databases to provide analytical insights and query-based responses in a booking data context. The API endpoints enable seamless access to insights and question answering, making the system robust for real-world deployment.
