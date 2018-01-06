# ubuntu_dialogs
Topic detection from chat logs of the Ubuntu dialog dataset

To run this code, please clone this repository onto your system with - 
``` $ git clone https://github.com/suki2691/ubuntu_dialogs.git ```

Setup-
* Python 3 (Anaconda)
* Scikit Learn package
* Jupyter Notebook
* NLTK
* LDA
* NMF
* Count Vectorizer
* TFIDF Vectorizer

Resources-
- Dataset - [data folder](https://github.com/suki2691/ubuntu_dialogs/tree/master/data)
- Code - [ubuntu_topic_detection](https://github.com/suki2691/ubuntu_dialogs/blob/master/Ubuntu_topic_detection.ipynb)
- Time spent- 1 day

## Approaches-

### Data Storage-
- I tried to store all the chat logs into a dataframe initially- it was inefficient since it took several hours to load all the chat data from all the files
- Then I tried to store the chat data into a list- all the chat data was loaded into a list within a few minutes (~ 1 million messages)

### Data Cleaning-
- I noticed that many of the messages had references of user names. So I found the set of distinct user names and added that set to the stop words list from nltk to create a new stop words list. This took care of user names appearing in the chat logs
- I removed all punctuation and special characters other than the period and the hyphen in both the user names and the chat messages

### Popular topics-
- To extract the most popular topics from all the messages, I tried to Count Vectorize the messages and then pass it through an LDA model. This approach did not give good results for 2 reasons-
  - When I Count Vectorized all the messages, words like 'somebody' and 'help' were chosen as the most popular topics
  - The LDA model took ~3 hours to fit on the entire dataset
- To remedy the above problems, I replaced the Count Vectorizer with TFIDF vectorizer and the LDA model with the NMF. The resulting topics were much closer to expectation and the NMF model fit the entire dataset in ~5 minutes
  - TFIDF downweighted the most commonly occuring words and gave better topic results
  - Even then, the most popular words were verbs like 'trying', 'working'
- I then extracted only the nouns from each sentence and discarded the other words. I counted the occurence of each noun in the entire dataset and stored it as a dictionary
  - The results from the above method was much better than NMF, but it did not make sense.
  - Why didn't it make sense? Because then the most popular topics became words like 'install' and 'ubuntu' - not very helpful to the Ubuntu help team
- I created bigrams from each sentence's nouns so that the topics make more sense to the Ubuntu help team
  - One of the popular topics was 'apt-get, install' 
  - I need to identify more words to add to the stop words matrix so that the output of the most popular topics does't contain fragmented words like don't and I'm

## Approach used for Question 2-
- Removed random user name references from the chat data of the given file
- Extracted the nouns from the sentences from the given dialog file
- Fit an NMF model to identify the main topics
  
## Next Steps-
- Clean the chat logs better to remove fragmented words like don't and I'm
- Some verbs are not removed. So, look for other POS tagging packages that are more efficient
- Add more common words to the stop words list- like somebody and help
