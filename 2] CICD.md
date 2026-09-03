# Project structure
        
        ai-cicd/
        │
        ├── model.py
        ├── test_model.py
        ├── requirements.txt
        └── .github/
            └── workflows/
                └── ci-cd.yml

# Create model.py

          from sklearn.linear_model import LinearRegression
          
          # Training data
          X = [[1], [2], [3], [4], [5]]
          y = [2, 4, 6, 8, 10]
          
          # Create and train model
          model = LinearRegression()
          model.fit(X, y)
          
          # Prediction
          prediction = model.predict([[6]])
          
          print("Prediction:", prediction[0])

- Run:

        python model.py

- Output will be approximately:

          Prediction: 12.0


# Create test_model.py

        from model import model
        
        def test_prediction():
            prediction = model.predict([[5]])
        
            assert round(prediction[0]) == 10

- This checks whether our AI model gives the expected prediction.

# Create requirements.txt

      scikit-learn
      pytest

- Install:

        pip install -r requirements.txt



# OR

          python -m pip install pytest
          python -m pytest --version

- Then run your tests:

          python -m pytest


# Create GitHub Actions Pipeline

- Create this file:

        .github/workflows/ci-cd.yml

- Add:

        name: AI CI/CD Pipeline
        
        on:
          push:
            branches: [main]
        
        jobs:
          test:
            runs-on: ubuntu-latest
        
            steps:
              - name: Get Code
                uses: actions/checkout@v4
        
              - name: Install Python
                uses: actions/setup-python@v5
                with:
                  python-version: "3.11"
        
              - name: Install Dependencies
                run: pip install -r requirements.txt
        
              - name: Run Tests
                run: pytest

# Push to GitHub
        
        git init
        git add .
        git commit -m "AI CI/CD project"
        git branch -M main
        git remote add origin YOUR_GITHUB_URL
        git push -u origin main

After pushing, GitHub automatically runs:

Code pushed
     ↓
GitHub Actions
     ↓
Install Python
     ↓
Install Libraries
     ↓
Run pytest
     ↓
    Test
   ↙    ↘
PASS    FAIL
 ↓       ↓
Done    Stop
⭐ In one line

Developer → GitHub → GitHub Actions → Install Dependencies → Test AI Model → Pass/Fail

This is a very simple CI pipeline for an AI application, without FastAPI or Docker.
