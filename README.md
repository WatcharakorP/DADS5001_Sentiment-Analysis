# DADS5001_Sentiment-Analysis


![image](https://github.com/WatcharakorP/DADS5001_Sentiment-Analysis/blob/main/header_project.JPG)

## 🚩 Introduction: 
* **`Livestreaming`** is streaming media simultaneously recorded and broadcast in real-time over the Internet.<br>
It is often referred to simply as streaming such as Youtube, Facebook, Twitch, TikTok, etc.

* **`Sentiment Analysis`** is the process of determine whether data is positive, negative or neutral.<br>
Sentiment analysis helps to monitor brand and product sentiment in customer feedback, and understand customer needs.

## 🎯 Diagram: 
* Collected data from comment of youtube livestream
* Sentiment data and Storage data into MySql database
* Visualized the streamed by Grafana
![image](https://github.com/WatcharakorP/DADS5001_Sentiment-Analysis/blob/main/Diagram.jpg)

## ✨Model Performance:
  This project will be using library from  **`Hugging Face`** to sentimental analysis.To determine whether comment on youtube is positive or negative 

## 📝 Source code:

```python
import pytchat
import mysql.connector
from transformers import pipeline
#from colorama import Fore,Style

def insertMySql(datetime,author,message,sentiment,score,ct_sentiment,ct_comment):

    cnx = mysql.connector.connect(user='sql6589589', password='DDFzTtZehi',host='sql6.freesqldatabase.com',database='sql6589589')

    cursor = cnx.cursor()

    add_record = ("INSERT INTO youtube_liveChat "
                "( datetime, author, message, sentiment, score, ct_sentiment, ct_comment) "
                "VALUES ( '"+datetime+"', '"+author+"', '"+message+"', '"+sentiment+"', '"+score+"','"+ct_sentiment+"','"+ct_comment+"')")
    

    # Insert new employee
    cursor.execute(add_record)
    
    cnx.commit()

    cursor.close()
    cnx.close()   
    print("Saved to Database") 


def main():

    classifier = pipeline('sentiment-analysis')

    chat = pytchat.create(video_id="LCb27GWfK6o")

    x=1
    y=1
    z=1

    while chat.is_alive():
        for c in chat.get().sync_items():
            #print(f'{c.datetime} {c.author.name} {c.message}')
            sentiment = classifier(c.message)
            mylist = sentiment[0]
            score = mylist['score']
            sentclass = mylist['label']
            
            if(sentclass == "NEGATIVE"):
                print(f'authort: {c.author.name} sent: {c.message} score: {score} sentiment: {sentclass}')
                insertMySql(str(c.datetime), str(c.author.name), str(c.message), str(sentclass), str(score), str(x), str(z))
                x += 1
            else:
                print(f'authort: {c.author.name} sent: {c.message} score: {score} sentiment: {sentclass}')
                insertMySql(str(c.datetime), str(c.author.name), str(c.message), str(sentclass), str(score), str(y), str(z))
                y += 1
            z += 1

if __name__ == "__main__":
    main()
```

## 📉 Visualization:
![image](https://github.com/WatcharakorP/DADS5001_Sentiment-Analysis/blob/main/grafana%20visualization.JPG)


## 🙋Member:
 **`6420422006`**  | 👦 **Watcharakorn Pasanta** 

## End credit: 
**This project is a part of Course DADS5001, Data Analytics and Data Science, NIDA.**





