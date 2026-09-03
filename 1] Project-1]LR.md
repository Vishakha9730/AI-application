# 1.Create a project folder
- Create a folder anywhere on your computer.
- For example:

      C:\Users\YourName\Desktop\docker-ai-lab

- You can also create it directly from VS Code.
- Go to:

    File → Open Folder

Create/select:

    docker-ai-lab

# Create the project structure
- Inside VS Code, create these files:

      docker-ai-lab/
      │
      ├── app.py
      ├── train.py
      ├── requirements.txt
      ├── Dockerfile
      └── .dockerignore

- After completing the lab, your project will look like:

      docker-ai-lab
      │
      ├── app.py
      ├── train.py
      ├── model.pkl
      ├── requirements.txt
      ├── Dockerfile
      └── .dockerignore

- model.pkl will be generated automatically.
- model.pkl is the saved version of your trained Machine Learning model.


        # Create the ML model
        - Open:train.py
        
        from sklearn.linear_model import LinearRegression
        import pickle
        
        # Training data
        X = [[1], [2], [3], [4], [5]]
        y = [10, 20, 30, 40, 50]
        
        # Create model
        model = LinearRegression()
        
        # Train model
        model.fit(X, y)
        
        # Save model
        with open("model.pkl", "wb") as file:
            pickle.dump(model, file)
        
        print("Model trained and saved successfully.")


  - pickle is a Python module used to save Python objects into a file and load them later.


# Create a Python virtual environment
- This step is useful for local development. Open the VS Code terminal:

      Terminal → New Terminal

- Run:

      python -m venv venv

- This creates:

          docker-ai-lab/
          │
          ├── venv/
          ├── app.py
          ├── train.py
          └── ...
 

# Activate the virtual environment
- Windows PowerShell

        .\venv\Scripts\Activate.ps1

- If you are using Command Prompt:

        venv\Scripts\activate

- You should see something like:

        (venv) PS C:\...\docker-ai-lab>

- The (venv) means the virtual environment is active.

# Install Python dependencies locally
- Run:

        pip install fastapi uvicorn scikit-learn

- These packages are needed for our application.
- What are they?
- These three tools work together to turn your Machine Learning model into an API that can run inside Docker.
- FastAPI - Used to create the API. FastAPI is a Python framework used to build web APIs. An API (Application Programming Interface) allows another application or user to communicate with your Python program.
- For example, suppose your ML model predicts a value. Without an API, you might directly write:

      prediction = model.predict([[6]])
      print(prediction)

- But if another application wants to use your model, it needs a way to send data to your Python application. That's where FastAPI comes in.
- Uvicorn - Runs the FastAPI application. FastAPI creates your API, but it needs a server to actually run and receive HTTP requests. That's where Uvicorn comes in. You can start your FastAPI application using:

      uvicorn app:app --host 0.0.0.0 --port 8000

- Let's break this down:

        uvicorn→ Start the Uvicorn server.

        app:app
        → First app = Python file app.py
        → Second app = FastAPI object:

        app = FastAPI()
        
        So:
        
        app:app
         │   │
         │   └── FastAPI application object
         └────── app.py

- Then:

        --host 0.0.0.0

- means the server can accept connections from outside the container.
- And:

        --port 8000

- means the application listens on port 8000.
- Scikit-learn - Provides our machine-learning model.

# Create requirements.txt
- Run:

      pip freeze > requirements.txt

- This creates the dependency file.
- Open: requirements.txt
- You will see packages and versions.

      For example:
      
      fastapi==...
      uvicorn==...
      scikit-learn==...

- There may also be additional dependencies required by scikit-learn.

# Train the model
- Run:

        python train.py

- Expected output:

          Model trained and saved successfully.

- Now check your VS Code Explorer.
- You should have:

        docker-ai-lab/
        │
        ├── app.py
        ├── train.py
        ├── model.pkl
        ├── requirements.txt
        ├── Dockerfile
        └── .dockerignore


- The important new file is:

      model.pkl

- This is our trained machine-learning model.

# Create the FastAPI application
- Open: app.py
- Add:

          from fastapi import FastAPI
          import pickle
          
          app = FastAPI()
          
          # Load the trained model
          with open("model.pkl", "rb") as file:
              model = pickle.load(file)
          
          
          @app.get("/")
          def home():
              return {
                  "message": "AI Prediction API is running"
              }
          
          
          @app.get("/predict")
          def predict(value: float):
          
              prediction = model.predict([[value]])
          
              return {
                  "input": value,
                  "prediction": prediction[0]
              }


# Understand the application
- When the user opens: /
this function runs:

    @app.get("/")
    def home():
        return {
            "message": "AI Prediction API is running"
        }

- When the user opens: /predict
- the model makes a prediction:

          @app.get("/predict")
          def predict(value: float):
          
              prediction = model.predict([[value]])
          
          For example:
          
          /predict?value=6
          
          The model predicts approximately:
          
          60
          
          because our training relationship is:
          
          1 → 10
          2 → 20
          3 → 30
          4 → 40
          5 → 50


# Test the application WITHOUT Docker
- Before containerizing the application, test that the Python application itself works.
- Run:

      uvicorn app:app --reload

- You should see something similar to:
- Uvicorn running on http://127.0.0.1:8000
- Open your browser:

        http://127.0.0.1:8000

- You should see:

      {
        "message": "AI Prediction API is running"
      }


# Test the prediction API
- Open:

        http://127.0.0.1:8000/predict?value=6

- You should get something similar to:

          {
            "input": 6,
            "prediction": 60.0
          }

# Open FastAPI documentation
- FastAPI automatically provides Swagger documentation.
- Open:

        http://127.0.0.1:8000/docs

- You should see the interactive API documentation.
- You can expand:

        GET /predict

- Then click:

        Try it out

- Enter:

        6

- and click:

        Execute

 
 # Stop the local application
- Go back to your VS Code terminal.
- Press:

        CTRL + C

- The local FastAPI server will stop.
- Now we are ready for Docker.

# Create the Dockerfile
- This is the most important file in the Docker lab.
- Create: Dockerfile
- Make sure it is exactly: Dockerfile not: Dockerfile.txt
- Add:

          FROM python:3.11-slim
          
          WORKDIR /app
          
          COPY requirements.txt .
          
          RUN pip install --no-cache-dir -r requirements.txt
          
          COPY . .
          
          EXPOSE 8000
          
          CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8000"]


# Understand every Dockerfile instruction
-  FROM: FROM python:3.11-slim
      - This tells Docker to use a Python 3.11 base image.
      - Instead of manually installing Python inside the container, Docker starts with a Python environment.

- WORKDIR: WORKDIR /app
    - Creates/sets the working directory inside the container.
    - Our application will live here: /app

- COPY

      COPY requirements.txt .

- It copies the requirements.txt file from your computer (project folder) into the Docker container's current working directory.

- RUN

        RUN pip install --no-cache-dir -r requirements.txt

      - Installs all Python dependencies inside the Docker image.

- Second COPY

        COPY . .

    - Copies our project files into the Docker image. This copies all files and folders from your project folder into the Docker container's current working directory.

- EXPOSE

          EXPOSE 8000

    - Documents that the application uses port 8000.

- CMD

        CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8000"]

- This starts our FastAPI application when the container starts.

# Create .dockerignore
- Create: .dockerignore
- Add:

      venv
      __pycache__
      *.pyc
      .git
      .vscode
      .env

- This tells Docker do not copy these files/folders into the Docker build context.
- Especially important: venv
- We don't want to copy our local Python virtual environment into the container because Docker creates its own environment.

# Your final project structure
- At this point:

        docker-ai-lab/
        │
        ├── app.py
        ├── train.py
        ├── model.pkl
        ├── requirements.txt
        ├── Dockerfile
        └── .dockerignore

- You may still have: venv/
- locally, but .dockerignore prevents it from being copied into the image.

# Build the Docker image
- Make sure Docker Desktop is running.
- In VS Code terminal, make sure you're inside: docker-ai-lab
- Run:

        docker build -t ai-prediction-app .

- Build an image.
- -t ai-prediction-app - Give the image a name.
- . - Use the current directory as the build context.

# Check your Docker image
  
      docker images


# Run the Docker container

        docker run -p 8000:8000 ai-prediction-app

#  Test the containerized application

      http://localhost:8000

- You should get:

        {
          "message": "AI Prediction API is running"
        }
        
        Congratulations! 🎉
        
        Your application is now running inside Docker.

# Test the ML prediction


        http://localhost:8000/predict?value=7

- Expected result:
          
          {
            "input": 7,
            "prediction": 70.0
          }

- The important point is that the prediction is now being generated by the ML model running inside the Docker container.

# Test Swagger

      http://localhost:8000/docs

- You can test the API from Swagger.

# Run the container in the background
- Currently, the terminal is occupied by the running container.
- Stop it: CTRL + C

      docker run -d -p 8000:8000 --name ai-container ai-prediction-app

# Check running containers

    docker ps


# View container logs

      docker logs ai-container

- You should see Uvicorn startup information.
- This is extremely useful when debugging containerized applications.

