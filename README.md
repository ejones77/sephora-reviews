# sephora-reviews
Using a dataset of product reviews, benchmark 3 vector databases on CRUD operations

## Project Overview
This project benchmarks Pinecone, MongoDB Atlas, and Elasticsearch for vector similarity search operations using Sephora product review data. The analysis includes data loading, querying, and performance comparisons across different vector database providers.

*This project was completed in December 2023 as part of the Masters in Data Science program at Northwestern University*

## Methods

### Dataset & Setup
- **Source**: 90,000 Sephora product reviews from Kaggle (cleaned from 969K records)
- **Embeddings**: `multi-qa-MiniLM-L6-cos-v1` model for semantic search
- **Platform**: All databases hosted on Google Cloud Platform

### Evaluation
- **CRUD Operations**: Timed bulk insert (90K records), queries, updates (5 records), and deletions
- **Test Queries**: 6 scenarios including natural language search, filtered queries, and similarity matching
- **Assessment**: Performance, ease of use, filtering capabilities, and cost analysis

### Database Configurations
- **Pinecone**: 384-dimension index, cosine similarity
- **MongoDB Atlas**: Vector search index on "values" field
- **Elasticsearch**: Script-score queries with cosine similarity

## Results

**Winner: MongoDB Atlas** - Best overall balance of performance, functionality, and cost.

### Performance Highlights
- **Create**: Pinecone fastest (~210s), Elasticsearch slowest (~541s)
- **Read**: MongoDB won 5/6 test queries, all databases <2s average
- **Update**: Elasticsearch fastest, MongoDB slowest (~5s)
- **Delete**: Elasticsearch fastest (<1s), MongoDB slowest (~21s)

### Key Differentiators
- **MongoDB**: Superior filtering and metadata handling
- **Pinecone**: Limited filtering, vectors separate from metadata  
- **Elasticsearch**: Good filtering but no native sorting in vector queries

### Cost Ranking (8GB dataset estimate)
1. Elasticsearch (most cost-effective)
2. MongoDB 
3. Pinecone (most expensive)

### Data Quality Challenges
- **Missing Products**: Expected shampoo products not found in dataset (web scraping limitations)
- **Sentiment Analysis**: Model returned positive reviews even for "worst product" queries
- **Dataset Bias**: Low volume of negative ratings overall

### Recommendation
**MongoDB Atlas** is recommended as the optimal choice for production vector search applications, based on:
- Balanced performance across all CRUD operations
- Superior filtering and metadata handling
- Cost-effective pricing structure
- Mature ecosystem with strong community support

## Setup 

### Prerequisites
- [Install uv](https://docs.astral.sh/uv/getting-started/installation/) for easy install of dependencies if not already available
- Python >= 3.12

### Dependencies
Install the project dependencies:
```bash
uv pip install -r pyproject.toml
```

### Data Setup
1. Download the Sephora dataset from [Kaggle](https://www.kaggle.com/datasets/nadyinky/sephora-products-and-skincare-reviews)
2. Place the `.csv` files in the main directory of this repo

### Environment Configuration
1. Copy `.env_example` to `.env`:
   ```bash
   cp .env_example .env
   ```

2. Fill in your database credentials in the `.env` file (see Vector Database Setup below)

## Vector Database Setup

### Pinecone
1. Sign up for [Pinecone](https://www.pinecone.io/)
2. Create a new project and get your API key
3. Note your environment (e.g., "gcp-starter")
4. Add to `.env`:
   ```
   PINECONE_API_KEY=your_api_key_here
   PINECONE_ENV=your_environment_here
   ```

### MongoDB Atlas
1. Sign up for [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
2. Create a new cluster
3. Create a database user and get the connection string
4. Enable vector search on your cluster
5. Create a vector search index named "sephora-index" on the "values" field
6. Add to `.env`:
   ```
   MONGO_USERNAME=your_username_here
   MONGO_PASSWORD=your_password_here
   MONGO_URI=your_cluster_uri_here
   ```

### Elasticsearch
1. Set up an Elasticsearch cluster (Cloud or self-hosted)
2. Generate an API key for authentication
3. Get your cluster URL
4. Add to `.env`:
   ```
   ELASTIC_API_KEY=your_api_key_here
   ELASTIC_URL=your_cluster_url_here
   ```

## Usage

### Running the Notebooks
1. **dataset-setup.ipynb**: Data cleaning and embedding generation
2. **data-loading.ipynb**: Load data into all three vector databases
3. **test-queries.ipynb**: Run benchmark queries and compare performance

### Index Configuration
- **Pinecone**: Creates index with 384 dimensions, cosine similarity
- **MongoDB**: Requires vector search index named "sephora-index" on "values" field
- **Elasticsearch**: Uses script_score with cosine similarity

## Project Structure
- `dataset-setup.ipynb`: Data preparation and embedding generation
- `data-loading.ipynb`: Database loading and initial setup
- `test-queries.ipynb`: Query benchmarking and performance testing
- `pyproject.toml`: Project dependencies
- `.env_example`: Environment variable template