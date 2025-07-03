These notebooks describe the various data collected and how it was presented. 

The data was collected using the standard reddit api (PRAW) that grabbed as much as it could without erroring out due to rate limit. Other than that, very basic clean up such as removing [deleted] users and comments.

Binning was performed using the 'created_utc' after converting using pd.to_datetime, by the code:

```python
all_time_bins = pd.concat([
    r_conservative['created_utc'],
    r_democrats['created_utc'],
    r_libertarian['created_utc'],
    r_politics['created_utc']
]).dt.floor('1H')
global_time_index = pd.date_range(all_time_bins.min(), all_time_bins.max(), freq='1H')
```

and stance classification by:

```python
def classify_stance(df, text_column='body', batch_size=32):
    model.eval()
    results = []
    with torch.no_grad():
        for i in range(0, len(df), batch_size):
            batch_texts = df[text_column].iloc[i:i+batch_size].tolist()
            inputs = tokenizer(batch_texts, padding=True, truncation=True, return_tensors="pt", max_length=512)
            outputs = model(**{k: v for k, v in inputs.items()})
            probs = torch.softmax(outputs.logits, dim=1)
            preds = torch.argmax(probs, dim=1).cpu().numpy()
            results.extend(preds)
    return results
```
with prepending the topic sentence for classification with topic for that specific output. 
