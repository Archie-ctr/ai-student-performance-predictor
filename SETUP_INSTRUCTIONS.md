# Setup Instructions

Complete guide to set up and run the AI Student Performance Predictor project.

## Prerequisites

- **Python 3.9+** - [Download Python](https://www.python.org/downloads/)
- **Git** - [Download Git](https://git-scm.com/downloads)

## Step 1: Clone the Repository

```bash
git clone https://github.com/Archie-ctr/ai-student-performance-predictor.git
cd ai-student-performance-predictor
```

## Step 2: Create Virtual Environment

### On macOS/Linux:
```bash
python3 -m venv venv
source venv/bin/activate
```

### On Windows:
```bash
python -m venv venv
venv\Scripts\activate
```

## Step 3: Install Dependencies

```bash
pip install --upgrade pip
pip install -r requirements.txt
```

## Step 4: Setup Environment Variables

Copy the example environment file:

```bash
cp .env.example .env
```

## Step 5: Prepare Data

Place your student performance data CSV file in `data/raw/` directory:

```
data/raw/student_data.csv
```

### Required CSV Columns:

```csv
attendance,study_hours,previous_gpa,assignment_score,midterm_score,final_grade
85,10,3.5,88,82,82.5
90,12,3.8,92,88,88.0
75,6,2.9,75,70,72.0
```

### Generate Sample Data (Optional)

```python
import pandas as pd
import numpy as np

np.random.seed(42)
n_students = 100

data = {
    'attendance': np.random.uniform(60, 100, n_students),
    'study_hours': np.random.uniform(2, 20, n_students),
    'previous_gpa': np.random.uniform(2.0, 4.0, n_students),
    'assignment_score': np.random.uniform(50, 100, n_students),
    'midterm_score': np.random.uniform(50, 100, n_students),
    'final_grade': np.random.uniform(50, 100, n_students),
}

df = pd.DataFrame(data)
df.to_csv('data/raw/student_data.csv', index=False)
print("Sample data created!")
```

## Step 6: Run the API Server

```bash
uvicorn src.api.main:app --reload
```

The API will be available at `http://localhost:8000`

## Step 7: Access the API

Open your browser and navigate to:

- **API Docs (Swagger UI)**: http://localhost:8000/docs
- **Alternative Docs (ReDoc)**: http://localhost:8000/redoc
- **Health Check**: http://localhost:8000/health

## Testing Predictions

### Using Swagger UI (Recommended)

1. Go to http://localhost:8000/docs
2. Click on `POST /api/predict`
3. Click "Try it out"
4. Enter sample data:

```json
{
  "attendance": 85,
  "study_hours": 10,
  "previous_gpa": 3.5,
  "assignment_score": 88,
  "midterm_score": 82
}
```

5. Click "Execute"

### Using cURL

```bash
curl -X POST "http://localhost:8000/api/predict" \
  -H "Content-Type: application/json" \
  -d '{
    "attendance": 85,
    "study_hours": 10,
    "previous_gpa": 3.5,
    "assignment_score": 88,
    "midterm_score": 82
  }'
```

### Using Python

```python
import requests

url = "http://localhost:8000/api/predict"
data = {
    "attendance": 85,
    "study_hours": 10,
    "previous_gpa": 3.5,
    "assignment_score": 88,
    "midterm_score": 82
}

response = requests.post(url, json=data)
print(response.json())
```

## Running Tests

```bash
pytest tests/ -v
```

## Common Issues & Troubleshooting

### Issue: `ModuleNotFoundError: No module named 'src'`

**Solution**: Run commands from the project root directory

### Issue: `FileNotFoundError: data/raw/student_data.csv`

**Solution**: Ensure the CSV file exists in `data/raw/` directory

### Issue: Port 8000 already in use

**Solution**: Use a different port:
```bash
uvicorn src.api.main:app --reload --port 8001
```

### Issue: Virtual environment not activated

**Solution**: Activate it again:
```bash
# macOS/Linux
source venv/bin/activate

# Windows
venv\Scripts\activate
```

## Next Steps

1. **Explore the API**: Visit http://localhost:8000/docs
2. **Make predictions**: Use the `/api/predict` endpoint
3. **Check endpoints**: Use `/api/info` to see available endpoints
4. **Train model**: Create script to train on your data
5. **Review code**: Check the source files in `src/` directory

## Support

For issues, questions, or suggestions:
1. Check existing GitHub issues
2. Open a new issue with details
3. Include error messages and setup information

Happy predicting! 🎓