# Global Dialogues Model Category Prediction

This project builds machine learning models to categorize open-ended survey responses from the Global Dialogues dataset. The system trains on manually labeled data and predicts categories for a larger unlabeled dataset.

## Project Overview

The project processes survey questions (D1Q1, D1Q2, D1Q3, D1Q4 for GDC1; D2Q1, D2Q2, D2Q3, D2Q4 for GDC2; and D4Q1 for GDC4) and builds multi-label classification models to predict response categories. It includes:

- **Enhanced Pipeline**: Multiple ML models (Logistic Regression, Random Forest, SVM, Neural Network, XGBoost)
- **Top-K Predictions**: Limits predictions to 1-2 categories per response for interpretability
- **Comprehensive Evaluation**: Detailed metrics and model comparison
- **Consolidated Output**: All predictions for each dataset in a single file for easy analysis
- **Interactive Web App**: Streamlit app for real-time question answering and category prediction
- **Persona Analysis**: Advanced user clustering and persona prediction system

## Dataset Details

### GDC1 (AI Ethics & Culture)
- **Questions**: D1Q1, D1Q2, D1Q3, D4Q4
- **Focus**: AI ethics, cultural perspectives, and moral principles
- **Training Data**: Manually labeled responses for each question

### GDC2 (AI Assistant Preferences)  
- **Questions**: D2Q1, D2Q2, D2Q3, D2Q4
- **Focus**: AI assistant personality, interaction style, and user preferences
- **Training Data**: Manually labeled responses for each question

### GDC4 (AI Impact)
- **Questions**: D4Q1
- **Focus**: Daily life changes due to AI in the past year
- **Training Data**: Fully labeled dataset with 1044 responses
- **Categories**: 12 categories including Information & Learning, Work & Productivity, Daily Tasks & Personal Management, etc.
- **Special Handling**: Since GDC4 is fully labeled, the consolidation process copies the Labels column directly to Predicted Categories

## Directory Structure

- `data/` — Input files (raw survey responses and labeled training data)
- `output/` — All generated files (predictions, models, consolidated outputs, persona models)
- `src/` — Source code for model training and prediction
  - `util/` — Utility functions for data processing, text cleaning, and persona utilities
  - `model_training/` — Model training and prediction scripts
  - `app/` — Interactive Streamlit web application
  - `persona_app/` — Persona prediction Streamlit application
  - `persona_creation/` — Scripts for creating persona models
- `tests/` — Test suite for model training and persona functionality

## Installation

1. **Clone the repository and set up virtual environment:**
```bash
git clone <repository-url>
cd global_dialogues_model_category
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

2. **Install dependencies:**
```bash
pip install -r requirements.txt
```

3. **Install Streamlit (for the web apps):**
```bash
pip install streamlit
```

## Generating Predictions

### 1. Run the Enhanced Pipeline for Each Question

For each question, run:

```bash
python src/model_training/enhanced_train_and_predict.py \
  --train "data/Categories - Data Sorting - D1Q1_ 1238 TOTAL RESPONSES.csv" \
  --predict "data/Categories - Data Sorting - GDC1.csv" \
  --output "output/GDC1_D1Q1_limited_predictions.csv" \
  --model "output/GDC1_D1Q1_model.pkl"
```

Repeat for D1Q2, D1Q3, D1Q4 (and similarly for D2Q1, D2Q2, D2Q3, D2Q4 with GDC2 files).

**For GDC4 (fully labeled data):**
```bash
python src/model_training/enhanced_train_and_predict.py \
  --train "data/Categories - Data Sorting - GDC4 - D4Q1_ 1044 TOTAL RESPONSES.csv" \
  --predict "data/Categories - Data Sorting - GDC4 - D4Q1_ 1044 TOTAL RESPONSES.csv" \
  --output "output/GDC4_D4Q1_limited_predictions.csv" \
  --model "output/GDC4_D4Q1_model.pkl"
```

### 2. Consolidate Predictions for All Questions

After generating all limited prediction files for a dataset, run:

**For GDC1:**
```bash
python src/model_training/consolidate_predictions.py --dataset GDC1
```

**For GDC2:**
```bash
python src/model_training/consolidate_predictions.py --dataset GDC2
```

**For GDC4:**
```bash
python src/model_training/consolidate_predictions.py --dataset GDC4
```

This will create:
- `output/GDC1_consolidated_predictions.csv` (for GDC1)
- `output/GDC2_consolidated_predictions.csv` (for GDC2)
- `output/GDC4_consolidated_predictions.csv` (for GDC4)

### 3. Output Files

- All `*_limited_predictions.csv` and `*_model.pkl` files are in `output/`
- Consolidated files are in `output/` as well

## Interactive Web Applications

### 1. Main Prediction App

After training the models, you can run the interactive web application:

```bash
streamlit run src/app/predict.py
```

The app will open in your default web browser at `http://localhost:8501`.

#### Features

- **Dataset Selection**: Choose between GDC1 (AI Ethics & Culture), GDC2 (AI Assistant Preferences), and GDC4 (AI Impact)
- **Question Selection**: Select from 4 questions per dataset (GDC1/GDC2) or 1 question (GDC4)
- **Interactive Answering**: Type your responses in a user-friendly interface
- **Real-time Prediction**: Get instant category predictions with confidence scores
- **Adjustable Parameters**: Modify confidence threshold and maximum categories
- **Export Results**: Download predictions as CSV files
- **Model Information**: View details about the trained models
- **Category Insights**: For each predicted category, see the percentage of users who gave a similar response and a representative example answer (drawn from the consolidated predictions).

#### Usage

1. Select a dataset (GDC1, GDC2, or GDC4)
2. Choose a question from the dropdown
3. Enter your response in the text area
4. Adjust prediction parameters if needed
5. Click "Predict Categories" to see results
6. Download results as CSV if desired
7. For each prediction, review the percentage of users with similar answers and see a real example from the dataset.

### 2. Persona Prediction App

The persona app provides a comprehensive user experience with a clean, professional design:

```bash
streamlit run src/persona_app/predict_persona.py
```

#### Features

- **Clean Professional Theme**: Modern, trustworthy design with Inter font family
- **Card-Based Layout**: Each question presented in clean white cards
- **Professional Color Palette**: Blue (#2563eb), gray (#64748b), and green (#10b981) scheme
- **Interactive Elements**: Hover effects, smooth animations, and focus states
- **Comprehensive Analysis**: Answer all 4 questions to get your complete persona profile
- **Visual Results**: Professional persona cards with confidence scores and user percentages
- **Similarity Chart**: Bar chart showing similarity to all available personas

#### Persona App Usage

1. Select a dataset (GDC1, GDC2, or GDC4)
2. Answer all 4 core questions in the card-based interface (GDC1/GDC2) or 1 question (GDC4)
3. Click "Find My Persona" to get your results
4. View your matched persona with confidence score and user percentage
5. Explore similarity to all other personas via the interactive chart

## Example Workflow

1. Generate predictions for all questions in GDC1, GDC2, and GDC4
2. Run the consolidation script for each dataset
3. Create persona models for all datasets
4. Analyze the consolidated output files
5. Use the Streamlit apps for interactive exploration

## Testing

Run the test suite to verify functionality:

```bash
python tests/model_training/test_enhanced_pipeline.py
```

To test the Streamlit app backend logic:
```bash
pytest tests/app/test_predict_app.py
```

To test persona functionality:
```bash
python tests/persona_app/test_persona_app.py
```

## Notes
- The consolidation script now supports GDC1, GDC2, and GDC4 via the `--dataset` argument.
- GDC4 is a fully labeled dataset with only one question (D4Q1), so the consolidation process simply copies the Labels column to Predicted Categories.
- All outputs are organized in the `output/` directory for clarity.
- The Streamlit apps require trained models in the `output/` directory to function properly.
- The Streamlit app's category statistics and example answer features require the consolidated predictions files (e.g., `output/GDC1_consolidated_predictions.csv`) to be present and up-to-date.
- The persona app uses precomputed persona models for fast, consistent results.

## Troubleshooting
- Ensure you have generated all required `*_limited_predictions.csv` files before running the consolidation script.
- If you add new datasets/questions, update the `DATASET_CONFIG` in `src/model_training/consolidate_predictions.py` accordingly.
- Make sure all model files are present in the `output/` directory before running the Streamlit apps.
- If the app shows "Model not loaded" errors, check that the model files exist and are accessible.
- For persona app issues, ensure persona model files (`*_persona_model.pkl`) are generated and present in the `output/` directory.

## Persona Creation and Persona App

### Overview

The project includes a robust persona modeling pipeline that clusters users into personas based on their predicted categories across all 4 core questions (for GDC1 and GDC2 datasets) or 1 question (for GDC4). Personas are created offline using the consolidated predictions, and the Streamlit persona app uses these precomputed persona models for fast, consistent user matching.

### How Persona Creation Works

1. **Input:**
   - Uses the consolidated predictions file for each dataset (e.g., `output/GDC1_consolidated_predictions.csv`).
   - Each user is represented as a vector of one-hot encoded predicted categories across all questions.

2. **Clustering:**
   - K-means clustering is used to group users into a configurable number of personas (default: 8).
   - Each persona centroid represents a typical pattern of category choices across questions.

3. **Persona Naming:**
   - For each persona, the top 2 dominant categories (across all questions) are identified.
   - Human-readable descriptions for these categories are used to generate a unique persona name (e.g., "The Honesty & Respect").
   - **Improved Naming**: Question codes (like "D2Q3_") are automatically removed from persona names for cleaner presentation.
   - If two personas would have the same name, a third (or more) dominant category is added to ensure all persona names are unique.
   - If a category code does not have a description, just the category code (e.g., "A") is used as a fallback.

4. **Output:**
   - All persona model data (centroids, names, descriptions, mappings, etc.) are saved as a `.pkl` file (e.g., `output/GDC1_persona_model.pkl`).

### How to Create Personas

Run the persona extraction script for each dataset:

```bash
./venv/bin/python src/persona_creation/extract_personas.py \
  --consolidated output/GDC1_consolidated_predictions.csv \
  --dataset GDC1 \
  --output output/GDC1_persona_model.pkl \
  --n_personas 8

./venv/bin/python src/persona_creation/extract_personas.py \
  --consolidated output/GDC2_consolidated_predictions.csv \
  --dataset GDC2 \
  --output output/GDC2_persona_model.pkl \
  --n_personas 8
```

### Using the Persona App

The Streamlit persona app (`src/persona_app/predict_persona.py`) loads the precomputed persona model files for GDC1 and GDC2. It does **not** recompute personas at runtime.

- Users answer the 4 core questions for the selected dataset in a clean, professional interface.
- The app predicts categories for each answer and builds a user vector.
- The user is matched to the closest persona(s) using cosine similarity to the persona centroids.
- The app displays the persona name (based on dominant categories), a description, confidence score, and user percentage.
- A similarity bar chart shows how the user compares to all available personas.

**Key Features:**
- **Clean Professional Design**: Modern UI with Inter font, professional color palette, and card-based layout
- **Unique Persona Names**: All persona names are guaranteed to be unique and meaningful
- **Visual Feedback**: Professional cards with hover effects and smooth animations
- **Comprehensive Analysis**: Complete persona matching with similarity comparisons

**Note:**
- Persona names/descriptions are always unique and based on the most representative categories for each cluster.
- Question codes are automatically cleaned from persona names for better presentation.
- If you update the consolidated predictions, you should rerun the persona extraction script to refresh the persona models.

---

For more details, see the comments in each script or contact the project maintainer. 