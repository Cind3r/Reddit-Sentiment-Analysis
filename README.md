# Reddit-Sentiment-Analysis
A Repo meant to store work for any sentiment context analysis done using the PRAW/HFT packages

# Analysis 1 - Political Subreddit responses to the bombing of Iran

#### Data Collection
Reddit comments were collected using the [PRAW (Python Reddit API Wrapper)](https://praw.readthedocs.io/) library. Specific threads related to the event of interest were identified in the following subreddits: Conservative, Democrats, Libertarians, and Politics. For each thread, all available comments were retrieved, including metadata such as author, score, timestamp, and subreddit.

#### Stance Classification
To analyze the stance expressed in each comment, I used a transformer-based model: [dominiks/stance-classification]. The model has typically been used in stance classifcation for tweets, not reddit comments. This model was accessed via the Sentence Transformers library. Two types of stance classification were performed:

- Stance (no topic): The model was applied to each comment without providing an explicit topic, resulting in a general stance label ('Support' or 'Oppose').
- Stance_topic (with topic): The same model was used, but with the relevant topic attached as context, allowing for stance classification relative to a specific issue or event.

#### Data Processing
- Comments from deleted or removed users were excluded from the analysis.
- For each subreddit, comments were grouped and analyzed by stance and by time (using hourly bins).
- Users posting in multiple subreddits were identified and anonymized for cross-community analysis.

#### Analysis
- The distribution of 'Support' and 'Oppose' stances was computed for each subreddit, both with and without topic context.
- Temporal trends were visualized using normalized stacked bar charts, showing how stances evolved over time.
- Pairwise comparisons and user-level analyses were conducted to explore cross-subreddit participation and stance consistency.

#### Cosine Similarity Matrix and Average Similarity Calculation
Text Preparation All comments from the dataframes are collected and combined with the topic statement to form a list of texts.

Embedding Generation Each text is converted into a vector representation (embedding) using a transformer model (dominiks/stance-detection). A fuction tokenizes the texts and extracts the [CLS] token embedding from the model's last hidden state.

Normalization Each embedding vector is normalized to unit length (L2 norm), resulting in the `normalized` array.

Cosine Similarity Matrix The cosine similarity between all pairs of normalized embeddings is computed using a dot product, resulting in the `similarity_matrix`. Each entry `[i, j]` in this matrix represents the similarity between text `i` and text `j`.

Average Similarity Calculation** The average similarity is calculated by summing all values in the similarity matrix, subtracting the diagonal (self-similarity), and dividing by the number of unique pairs of text

Average similarity between comments in the threads for Conservative, Democrats, and Libertarians with the given topic text was 0.739. The cosine similarity matrix heatmap can be seen below for anyone that cares:

![image](https://github.com/user-attachments/assets/a24c5dd4-00f5-415d-b002-04d04e7e00a8)

Links to Threads
- [Conservative Thread](https://www.reddit.com/r/Conservative/comments/1lhacy4/trump_announces_the_us_has_bombed_iran/)  
- [Democrats Thread](https://www.reddit.com/r/democrats/comments/1lhaml6/we_are_at_war/)  
- [Libertarian Thread](https://www.reddit.com/r/Libertarian/comments/1lhaujm/so_uh_are_we_going_to_war_with_iran/)  
- [Politics Thread](https://www.reddit.com/r/politics/comments/1lhaiqa/megathread_us_president_trump_says_that_the_us/)



![image](https://github.com/user-attachments/assets/49050f4a-fa87-4b7b-b3f8-40e4bee41e99)

#### r/Politics response over 24 hrs from initial post

![image](https://github.com/user-attachments/assets/8f98f1fd-e0ed-49b8-a2d9-4c6ff36fc0e0)

#### Users cross-posting in comments

Note: The total number of users that posted in more than 1 subreddit is 38, the total number of comments made by these users is 108. The pie charts and numbers that come off of them show the number of users that posted in the subreddit as well ass r/Politics (e.g. 3 users posted in r/Conservative and r/Politics, 60% of their comments were in r/Conservative). There isn't much data to work with here, but it is a quick way to check if the sentiment analysis was affected by 'brigading' and cross-posting. 

![image](https://github.com/user-attachments/assets/4c3771f3-d8b7-4db7-b7bc-d67da59c8d05)
