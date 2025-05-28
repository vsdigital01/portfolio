**Reddit Sentiment Analysis**
# This notebook analyzes the sentiment of Reddit posts from a chosen subreddit using the VADER sentiment analysis tool. It fetches posts via the Reddit API using `praw`, calculates sentiment scores, and visualizes the results.


```python
# 1. Install and Import Dependencies

!pip install praw nltk
import praw
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import nltk
from nltk.sentiment.vader import SentimentIntensityAnalyzer

# Download VADER data
nltk.download('vader_lexicon')
```

    Requirement already satisfied: praw in c:\users\veron\anaconda3\lib\site-packages (7.8.1)
    Requirement already satisfied: nltk in c:\users\veron\anaconda3\lib\site-packages (3.8.1)
    Requirement already satisfied: prawcore<3,>=2.4 in c:\users\veron\anaconda3\lib\site-packages (from praw) (2.4.0)
    Requirement already satisfied: update_checker>=0.18 in c:\users\veron\anaconda3\lib\site-packages (from praw) (0.18.0)
    Requirement already satisfied: websocket-client>=0.54.0 in c:\users\veron\anaconda3\lib\site-packages (from praw) (0.58.0)
    Requirement already satisfied: click in c:\users\veron\anaconda3\lib\site-packages (from nltk) (8.1.7)
    Requirement already satisfied: joblib in c:\users\veron\anaconda3\lib\site-packages (from nltk) (1.2.0)
    Requirement already satisfied: regex>=2021.8.3 in c:\users\veron\anaconda3\lib\site-packages (from nltk) (2023.10.3)
    Requirement already satisfied: tqdm in c:\users\veron\anaconda3\lib\site-packages (from nltk) (4.65.0)
    Requirement already satisfied: requests<3.0,>=2.6.0 in c:\users\veron\anaconda3\lib\site-packages (from prawcore<3,>=2.4->praw) (2.31.0)
    Requirement already satisfied: six in c:\users\veron\anaconda3\lib\site-packages (from websocket-client>=0.54.0->praw) (1.16.0)
    Requirement already satisfied: colorama in c:\users\veron\anaconda3\lib\site-packages (from click->nltk) (0.4.6)
    Requirement already satisfied: charset-normalizer<4,>=2 in c:\users\veron\anaconda3\lib\site-packages (from requests<3.0,>=2.6.0->prawcore<3,>=2.4->praw) (2.0.4)
    Requirement already satisfied: idna<4,>=2.5 in c:\users\veron\anaconda3\lib\site-packages (from requests<3.0,>=2.6.0->prawcore<3,>=2.4->praw) (3.4)
    Requirement already satisfied: urllib3<3,>=1.21.1 in c:\users\veron\anaconda3\lib\site-packages (from requests<3.0,>=2.6.0->prawcore<3,>=2.4->praw) (2.0.7)
    Requirement already satisfied: certifi>=2017.4.17 in c:\users\veron\anaconda3\lib\site-packages (from requests<3.0,>=2.6.0->prawcore<3,>=2.4->praw) (2024.6.2)
    

    [nltk_data] Downloading package vader_lexicon to
    [nltk_data]     C:\Users\veron\AppData\Roaming\nltk_data...
    [nltk_data]   Package vader_lexicon is already up-to-date!
    




    True




```python
# 2. Connect to Reddit API

reddit = praw.Reddit(
    client_id='MSKfwFbWQqiKgkITJuZxcg',
    client_secret='t_VLYfZuybrNzuTSvRdMuldMCzVZlA',
    user_agent='reddit_sentiment_app by /u/Party-Sea-3481'
)

# Test the connection
print("API connection successful:", reddit.read_only)
```

    API connection successful: True
    


```python
# 3. Fetch Hot Posts from a Subreddit

subreddit_name = "dataisbeautiful"
num_posts = 100

# Fetch posts
titles = []
for post in reddit.subreddit(subreddit_name).hot(limit=num_posts):
    titles.append(post.title)

# Create a DataFrame
df = pd.DataFrame(titles, columns=["title"])
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>title</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>[Topic][Open] Open Discussion Thread — Anybody...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>The (mental health) death iceberg - deaths due...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>[OC] My COVID Progression of Symptoms</td>
    </tr>
    <tr>
      <th>3</th>
      <td>[OC] The Importance of Regulation - US lead-cr...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>[OC] The Biggest Listed Companies in Japan</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 4. Perform Sentiment Analysis

# Initialize VADER sentiment analyzer
sid = SentimentIntensityAnalyzer()

# Get compound sentiment scores
df['compound_score'] = df['title'].apply(lambda x: sid.polarity_scores(x)['compound'])

# Categorize into labels
def get_sentiment_label(score):
    if score >= 0.05:
        return 'Positive'
    elif score <= -0.05:
        return 'Negative'
    else:
        return 'Neutral'

df['sentiment'] = df['compound_score'].apply(get_sentiment_label)
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>title</th>
      <th>compound_score</th>
      <th>sentiment</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>[Topic][Open] Open Discussion Thread — Anybody...</td>
      <td>0.3802</td>
      <td>Positive</td>
    </tr>
    <tr>
      <th>1</th>
      <td>The (mental health) death iceberg - deaths due...</td>
      <td>-0.9260</td>
      <td>Negative</td>
    </tr>
    <tr>
      <th>2</th>
      <td>[OC] My COVID Progression of Symptoms</td>
      <td>0.0000</td>
      <td>Neutral</td>
    </tr>
    <tr>
      <th>3</th>
      <td>[OC] The Importance of Regulation - US lead-cr...</td>
      <td>0.3612</td>
      <td>Positive</td>
    </tr>
    <tr>
      <th>4</th>
      <td>[OC] The Biggest Listed Companies in Japan</td>
      <td>0.0000</td>
      <td>Neutral</td>
    </tr>
  </tbody>
</table>
</div>




```python
## 5. Visualize Sentiment Distribution
sns.set(style="whitegrid")
plt.figure(figsize=(8,5))
sns.countplot(data=df, x='sentiment', palette='Set2')
plt.title(f"Sentiment Analysis of Top {num_posts} Posts from r/{subreddit_name}")
plt.xlabel("Sentiment")
plt.ylabel("Number of Posts")
plt.show()
```


    
![png](output_5_0.png)
    



```python

```
