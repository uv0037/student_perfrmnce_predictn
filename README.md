# Student Performance Prediction

This repository implements an end-to-end machine learning pipeline for predicting student performance based on multiple input features. The project is designed to be modular, with separate components for data ingestion, transformation, model training, and inference.


## Overview

The pipeline takes raw student data, processes it, trains a predictive model, and offers a web interface for predictions. The main stages are:

1. **Data Ingestion**: Load and split the raw dataset.
2. **Data Transformation**: Clean, encode, and scale the features.
3. **Model Training**: Train and evaluate the prediction model.
4. **Prediction**: Serve predictions via a Flask web app.


## Data Flow in the Pipeline

Below is a flowchart and step-by-step explanation showing how data moves through the pipeline:

```mermaid
graph TD
    A[Raw Data (stud.csv)] --> B[Data Ingestion]
    B -->|train/test split| C[train.csv & test.csv]
    C --> D[Data Transformation]
    D -->|preprocessing + feature engineering| E[Preprocessed Train/Test Sets]
    E --> F[Model Training]
    F -->|model.pkl| G[Prediction Pipeline]
    G --> H[Web Application]
    H --> I[User Inputs]
    I --> G
    G --> J[Predicted Score]
    J --> H
```

### Step-by-Step Flow

**1. Data Ingestion (`src/components/data_ingestion.py`):**
- Reads `stud.csv` and exports it as a DataFrame.
- Splits data into train and test sets (`train.csv`, `test.csv`).
- Stores the files in the `artifact/` directory.

**2. Data Transformation (`src/components/data_transformation.py`):**
- Reads train and test CSVs.
- Applies preprocessing pipelines:
  - *Numerical features*: Imputation + scaling.
  - *Categorical features*: Imputation + one-hot encoding + scaling.
- Saves the preprocessor object (`preprocess.pkl`).

**3. Model Training (`src/model_trainer.py`):**
- Trains the model using the transformed datasets.
- Stores the trained model as `model.pkl`.

**4. Prediction Pipeline (`src/pipeline/predict_pipeline.py`):**
- Loads the preprocessor and model.
- Transforms user input using the preprocessor.
- Predicts the student's performance.

**5. Web Application (`app.py`):**
- Accepts user input via a web form.
- Passes the data through the prediction pipeline.
- Displays the predicted score to the user.


## How to Run

1. **Clone the repository**

    ```bash
    git clone https://github.com/uv0037/student_perfrmnce_predictn.git
    ```

2. **Create a conda environment**

    ```bash
    conda create -n stud python=3.8 -y
    conda activate stud
    ```

3. **Install requirements**

    ```bash
    pip install -r requirements.txt
    ```

4. **Run the application**

    ```bash
    python app.py
    ```

5. **Access the web interface**

    Open your browser and go to `http://localhost:5000` (or the port displayed in your console).


## Files of Interest

- `src/components/data_ingestion.py`: Handles reading and splitting raw data.
- `src/components/data_transformation.py`: Performs feature engineering and preprocessing.
- `src/pipeline/predict_pipeline.py`: Orchestrates prediction logic.
- `app.py`: Flask web server for user interaction.


## Explanation & Traceability

- All steps log their progress for traceability (`src/logger.py`).
- Configuration files (`config.yaml`, `params.yaml`, `schema.yaml`) allow easy control of pipeline parameters.
- Artifacts including model and processed data are stored in the `artifact/` directory for reproducibility.


## Example Inputs

The model expects features such as:
- Gender
- Race/Ethnicity
- Parental level of education
- Lunch type
- Test preparation course
- Reading score
- Writing score

The web app form collects these and provides an instant prediction.


## License

This project is licensed under the MIT License.
