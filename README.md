# Twitter Bot Fraud Detection

Detecting Twitter bots using feature engineering and graph-based learning (RGCN).

---

## Pipeline Overview

1. Data Processing  
2. Feature Engineering  
3. Graph Construction  
4. Model Training and Evaluation  

---

## Project Structure

### 1. Sampling from Raw TwiBot22 Dataset

The following ipynb files are used to generate the reported performance.

**sampling.ipynb**
- Implements balanced seed-based expansion sampling on the raw graph dataset  
- Produces a final sample of ~40k users  
- Fetches relevant tweets for sampled users  
- Aligns tweet and user subsets with sampled graph nodes  

### 2. Feature Engineering

**fe_tweets.ipynb**  
- Cleans and preprocesses tweet text (regex, emoji handling)  
- Extracts semantic features using transformer models  
- Generates tweet-level behavioral and content features  

**fe_user_metadata.ipynb**  
- Cleans and structures user metadata  
- Handles missing values and inconsistencies  
- Produces user-level features (activity, profile statistics)  

### 3. Graph Construction & Modeling

**graph.ipynb**  
- Constructs the graph using engineered features  
- Trains a Relational Graph Convolutional Network (RGCN)  
- Evaluates model performance (Accuracy, Precision, Recall, F1-score)  

### Environment

**requirements.txt**  
- List of Python dependencies required to run the project  

---


## Dataset Description

### Raw Data Files

**1. edge.parquet**  
- Optimized edge list for efficient processing  
- Represents relationships between entities (user-user, user-tweet)  
- Contains source node, target node, and relation type  

**2. label.csv**  
- Ground truth labels for users  
- Binary classification (bot vs human)  

**3. split.csv**  
- Defines train, validation, and test splits for users

**4. user.json**  
- Raw user metadata  
- Includes profile attributes (followers, account info, activity stats)  

### Final Processed Outputs (`final_outputs/`)
These are intermediate datasets generated at different stages of the pipeline after sampling and feature engineering.  

**1. df_edges_final.parquet**  
- Final edge list after sampling  
- Contains only nodes in the training subgraph  
- Used for graph construction  

**2. df_tweets_filtered.csv**  
- Filtered tweets aligned with sampled users  
- Removes irrelevant or unused tweets  

**3. df_tweets_final.parquet**  
- parquet version of the csv file df_tweets_filtered.csv
- optimised for efficient processing

**4. df_tweets_model.parquet**  
- Model-ready tweet dataset  
- Contains processed and selected tweet-level features aligned with sampled users  
- Includes features used for downstream modeling (embeddings or aggregated tweet signals)  
- Serves as input for integrating tweet information into the graph model 
- optimised for efficient processing
 
**5. df_user_meta_full.parquet**  
- Cleaned and processed user metadata  
- Includes all users before sampling  

**6. df_users_final.parquet**  
- Sampled subset of users used in the graph  

**7. df_users_model.parquet**  
- Final model-ready user dataset  
- Includes user features, labels, and engineered features  
- Direct input into the graph model  

---

## Data Flow and File Relationships

The project follows a sequential pipeline where outputs from each stage serve as inputs to the next:

- **sampling.ipynb** selects a balanced subset of users (~40k) and extracts the corresponding edges, tweets, and user metadata  

- **fe_tweets.ipynb** processes sampled tweet data and generates tweet-level features  

- **fe_user_metadata.ipynb** cleans and prepares user-level features from metadata  

- **sampling + feature engineering outputs** are then aligned to ensure all datasets contain the same set of users  

- **graph.ipynb** combines multiple files to train and evaluate the RGCN model
  - User features (`df_users_model.parquet`)  
  - Tweet features (`df_tweets_model.csv`)  
  - Graph structure (`df_edges_final.parquet`)  


---

## How to Run

Run notebooks in the following order:

1. `sampling.ipynb`  
2. `fe_tweets.ipynb`  
3. `fe_user_metadata.ipynb`  
4. `graph.ipynb`  


## Objective

Classify users as bot or human using both:
- Individual features (tweets and metadata)  
- Network structure (user relationships)  