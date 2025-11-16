# 📈 Financial News NER (Named Entity Recognition)

A Python-based Named Entity Recognition (NER) system for extracting financial entities from news articles. Built with Hugging Face Transformers, this tool identifies stocks/tickers, companies, and financial events from text.

## 🎯 Features

- **Stock/Ticker Extraction**: Identifies stock symbols (e.g., `AAPL`, `TSLA`, `NVDA`)
- **Company Recognition**: Extracts company names using NER models (e.g., `Apple Inc.`, `Tesla`)
- **Event Detection**: Identifies financial events (earnings, mergers, dividends, IPOs, etc.)
- **Live News Integration**: Fetches and processes real-time news via NewsAPI.org
- **Batch Processing**: Efficient processing of multiple articles
- **Export Capabilities**: Save results as CSV or JSONL format

## 🚀 Quick Start

### Prerequisites

- Python 3.8+
- NewsAPI.org API key (free tier available at https://newsapi.org)

### Installation

```bash
pip install -U transformers torch pandas tqdm rich requests python-dateutil
pip install newsapi-python
```

### Basic Usage

```python
from transformers import pipeline

# Load the NER model
ner = pipeline("ner", model="dslim/bert-base-NER", grouped_entities=True)

# Extract entities from text
text = "Apple (AAPL) raised its Q4 guidance after strong iPhone sales."
result = extract_entities(text)

print("Companies:", result.companies)  # ['Apple']
print("Tickers:", result.tickers)      # ['AAPL']
print("Events:", result.events)        # ['guidance']
```

## 📊 Project Structure

```
fin-news-ner/
├── financial_news_ner.ipynb   # Main notebook with all functionality
└── README.md                   # This file
```

## 🔧 How It Works

### 1. NER Model
Uses the pre-trained `dslim/bert-base-NER` model from Hugging Face to identify organizations and entities.

### 2. Ticker Extraction
Employs regex patterns to identify stock ticker symbols (2-5 character uppercase strings).

### 3. Event Detection
Matches against a curated list of financial event keywords:
- `earnings`, `guidance`, `dividend`
- `merger`, `acquisition`, `buyback`
- `IPO`, `bankruptcy`, `layoff`
- `lawsuit`, `settlement`, `investigation`
- `partnership`, `forecast`

### 4. NewsAPI Integration
Fetches live financial news articles and processes them through the NER pipeline.

## 💡 Usage Examples

### Process Example Texts

```python
examples = [
    "Apple (AAPL) raised its Q4 guidance after strong iPhone sales.",
    "Tesla Inc. (TSLA) announced a $5B buyback.",
    "NVIDIA (NVDA) beats earnings and issues positive outlook."
]

results = [extract_entities(x) for x in examples]
```

### Fetch Live News

```python
# Set your NewsAPI key
import os
os.environ["NEWSAPI_KEY"] = "your_api_key_here"

# Fetch and process recent news
df_live = newsapi_ner_demo(
    query="earnings OR results OR guidance",
    page_size=20,
    hours_back=72
)

# View results
print(df_live.head())
```

### Export Results

```python
# Export to CSV and JSONL
export_results(df_live, stem="financial_news_results")
# Creates: financial_news_results.csv and financial_news_results.jsonl
```

## 📦 Key Functions

- `load_ner_pipeline()`: Initializes the NER model with compatibility for different API versions
- `extract_entities(text)`: Extracts companies, tickers, and events from a text string
- `process_batch(texts)`: Processes multiple texts and returns a pandas DataFrame
- `fetch_newsapi_articles()`: Fetches articles from NewsAPI.org
- `newsapi_ner_demo()`: End-to-end demo fetching and processing live news
- `export_results()`: Exports DataFrame to CSV and JSONL formats

## ⚠️ Limitations & Notes

### NewsAPI Free Tier
- Limited requests per day
- Restricted to specific news sources
- Consider implementing caching for repeated queries

### Model Accuracy
- Pre-trained model may miss domain-specific entities
- Ticker detection relies on regex patterns (may have false positives)
- Event detection uses simple keyword matching

### Production Recommendations
- Implement article de-duplication by URL
- Filter out irrelevant news sources
- Enrich ticker data with reference APIs (e.g., OpenFIGI)
- Consider fine-tuning the NER model on financial news datasets
- Add relation extraction for more sophisticated event understanding

## 🛠️ Technology Stack

- **Transformers**: Hugging Face library for NER models
- **PyTorch**: Deep learning framework
- **Pandas**: Data manipulation and analysis
- **NewsAPI**: Real-time news article fetching
- **Python-dateutil**: Date parsing utilities

## 📝 Output Format

The `NERResult` dataclass contains:
```python
@dataclass
class NERResult:
    text: str                           # Original input text
    companies: List[str]                # Extracted company names
    tickers: List[str]                  # Extracted stock tickers
    events: List[str]                   # Detected financial events
    raw_entities: List[Dict[str, Any]]  # Raw NER model output
```

## 🔮 Future Enhancements

- [ ] Fine-tune model on financial news corpus
- [ ] Add sentiment analysis for each entity
- [ ] Implement relationship extraction (company-event pairs)
- [ ] Support for international stock exchanges
- [ ] Real-time streaming data processing
- [ ] Web API endpoint deployment

## 📄 License

This project is open source and available for educational and commercial use.

## 🤝 Contributing

Contributions, issues, and feature requests are welcome!

## 📧 Contact

For questions or feedback, please open an issue on the repository.

---

**Note**: Remember to keep your NewsAPI key secure and never commit it to version control.