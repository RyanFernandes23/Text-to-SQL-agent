**Text-to-SQL Agent Performance Documentation**  
**Accuracy: 58.75%**  

---

### **Project Overview**  
A LangGraph-based agent converts natural language queries to SQL using Groq's Llama 3-70B and executes them against the Pagila database. The agent retries up to 3 times on errors, logs successful queries to `output_queries.txt`, and saves results to `results.txt`.

---

### **Setup Instructions**  
#### **Prerequisites**  
1. **Docker** (for PostgreSQL container)  
2. **Python 3.9+** with libraries:  
   ```bash
   pip install langgraph psycopg2 python-dotenv groq
   ```  
3. **Groq API Key** (set as environment variable `GROQ_API_KEY`)  

---

#### **Steps**  
1. **Clone Pagila Database**  
   ```bash
   git clone https://github.com/devrimgunduz/pagila.git
   cd pagila
   ```  

2. **Start PostgreSQL with Docker**  
   ```bash
   docker-compose up -d  # Launches Pagila database on port 5432
   ```  

3. **Initialize Database**  
   ```bash
   docker exec -it pagila psql -U postgres -c "CREATE DATABASE pagila;"
   docker exec -i pagila psql -U postgres pagila < pagila-schema.sql
   docker exec -i pagila psql -U postgres pagila < pagila-data.sql
   ```  

4. **Configure Database Connection**  
   In `main.ipynb`, update credentials:  
   ```python
   import psycopg2
   conn = psycopg2.connect(
       host="localhost",
       port=5432,
       user="postgres",
       password="postgres",  # Default Docker setup
       database="pagila"
   )
   ```  

5. **Run the Agent**  
   Execute `main.ipynb` to start processing natural language queries.  

---

### **Test Results & Analysis**  
*(Previous test cases and score breakdown remain unchanged from original documentation)*  

---

### **Architecture**  
![image alt](https://github.com/RyanFernandes23/Text-to-SQL-agent/blob/main/langgraph.png?raw=true)

---

### **Troubleshooting**  
1. **Docker Connection Issues**  
   - Verify container is running: `docker ps -a`  
   - Check logs: `docker logs pagila`  

2. **Empty Results**  
   - Confirm table names/aliases match Pagila schema (e.g., `city` vs `address.city_id`).  

3. **Groq API Errors**  
   - Ensure environment variable is set:  
     ```bash
     export GROQ_API_KEY="your-key-here"
     ```  

---

### **Limitations & Future Work**  
1. **Schema Awareness**  
   Current agent lacks explicit knowledge of Pagila's table relationships (e.g., `customer → address → city → country`).  

2. **Error-Driven Retries**  
   Future versions could parse PostgreSQL errors to guide SQL correction (e.g., missing column → suggest joins).  

3. **Performance Scaling**  
   Test with larger datasets like Pagila's 16k+ rental records.  

--- 

This enhanced documentation now provides full reproducibility while maintaining clarity about performance outcomes.
