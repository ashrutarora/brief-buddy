# Brief Buddy: news article summarizer

Brief Buddy is a Jupyter notebook that pulls the latest stories from the
[Hindustan Times India news page](https://www.hindustantimes.com/india-news) and writes a short summary of each one.

It is a small learning project: a scraper plus a pretrained summarization model. I did not train or fine-tune the
model myself.

## How it works

1. **Scrape the listing page.** `requests` and BeautifulSoup fetch the India news page and collect each story's link.
2. **Fetch each article.** For every link, the notebook reads the headline and joins the article's paragraphs into one text.
3. **Summarize.** The text goes to [`facebook/bart-large-cnn`](https://huggingface.co/facebook/bart-large-cnn), a BART model that
   Facebook AI already fine-tuned for news summarization on the CNN/DailyMail dataset. It is used as is, loaded from
   Hugging Face. Generation uses beam search (4 beams, up to 150 tokens).
4. **Print** each headline with its summary.

## Run it

```
pip install transformers torch beautifulsoup4 requests
git clone https://github.com/ashrutarora/brief-buddy.git
```

Open `Brief Buddy (Realtime–News-Summarizer).ipynb` and run the cells in order. The first run downloads the model
(about 1.6 GB).

## Limits

- The scraper depends on the page's HTML class names (`listingPage`, `cartHolder`, `hdg1`, `detail`). If Hindustan
  Times changes its markup, the scraping cells need new selectors.
- BART reads at most 1,024 tokens, so long articles are cut off before summarizing.
- Summaries are not evaluated (no ROUGE scores or human review), so their quality is unmeasured.
