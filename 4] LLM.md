# Step 1 — Install Ollama
- Download and install Ollama for Windows: Download Ollama
- After installing, restart VS Code.
- Open PowerShell and run:

      ollama --version

# Step 2— Download a small LLM
- For a beginner lab, use a small model:

            ollama pull llama3.2:1b

- This downloads the model to your computer. Then test it:

            ollama run llama3.2:1b

- Ask:
- What is artificial intelligence?
- If it gives you an answer, Ollama is working. To exit:

          /bye

# Step 3 — Install Python Library
- In your VS Code terminal:

      pip install ollama

# Step 4 — Create Your Project


        LLM-Monitoring
        │
        └── app.py

# Step 5 — Write Simple Python Code
- Open app.py:

          import ollama
          import time
          
          count = 0
          
          while True:
          
              question = input("\nAsk a question: ")
          
              if question.lower() == "exit":
                  break
          
              count = count + 1
          
              start_time = time.time()
          
              response = ollama.chat(
                  model="llama3.2:1b",
                  messages=[
                      {
                          "role": "user",
                          "content": question
                      }
                  ]
              )
          
              end_time = time.time()
          
              print("\nAI Answer:")
              print(response["message"]["content"])
          
              print("\n--- Monitoring ---")
              print("Request Number:", count)
              print("Response Time:", round(end_time - start_time, 2), "seconds")

# Step 6 — Run Your Application

        python app.py

- You will see:

      Ask a question:
      
      Enter:
      
      What is AI?

      You should get something like:
      
      AI Answer:
      Artificial Intelligence is...
      
      --- Monitoring ---
      Request Number: 1
      Response Time: 2.35 seconds

      Ask another question:
      
      What is machine learning?
      
      You might see:
      
      --- Monitoring ---
      Request Number: 2
      Response Time: 1.87 seconds

Step 9 — Stop the Program

      exit

