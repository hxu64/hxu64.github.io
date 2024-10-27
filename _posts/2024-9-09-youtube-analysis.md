---
title: YouTube Realtime Sentiment Analysis
description: >-
  Creating a solution that analyses comment sentiment in real time
author: 
date: 2024-9-09 20:55:00 +0800
categories: [Projects]
tags: [Database, Machine Learning]
pin: true
media_subpath: '/assets/img/youtubeanalysis/'
---

## Background

Leading up to Christmas break of 2024, I decided to create a [YouTube Channel](https://www.youtube.com/@halcyK) to gain experience creating content and to keep myself occupied over the break. Little did I know, this project would soon take up almost all of my time, and I didn't end up getting to interact with the community as much as I would've liked. I'll detail my experiences as a YouTuber in a future blog post.

In order to ensure that my content was well-received and to obtain feedback on my work, I needed to gather actionable insights from the comments section. Unfortunately because of [YouTube's decision to 'remove' dislikes](https://blog.youtube/news-and-events/update-to-youtube/), most videos recieve disporportionately more likes than dislikes, which makes the like-to-dislike ratio no longer a good metric.

Initially, it's feasiable for creators to read all of their comments. However, as your channel grows, it becomes much more difficult to keep up with your community, which is why I decided to take it up to myself to develope a solution for obtaining real-time actionable data from these comments, by implementing my own real-time sentiment analyzer for YouTube comments, to accurately guage the reception of my content.

## Architecture

![Architecture Image](youtube_analysis_architecture.png)

The overarching goal of this architecture is to capture and analyze real-time sentiment from YouTube comments. The flow involves collecting comments, processing them for sentiment analysis, and then delivering the results back to the user for actionable insights.
Producer

### Producer

The first step in this architecture involves obtaining YouTube comments through Google Cloud's YouTube API, which allows for streamlined way to access comment data associated with videos. Once the comments are retrieved, they are formatted into structured messages and published to a Kafka Topic. This ensures that comments can be processed in real time as they come in. 

### Broker

To handle the message transmission, I opted to set up Apache Kafka on a virtual machine running Ubuntu. Kafka serves as a distributed messaging system that allows for high-throughput data streaming. Within this setup, I created a Kafka topic, which functions similarly to a table in a database, to organize and store the incoming messages. To make the broker more reliable, I chose to run the topic over 3 ports, enabling the use of 3 bootstrapped servers. This is especially important, as if one server encounters an issue the other ones can continue working.

The use of Zookeeper in this architecture is crucial, making sure that that the system remains scalable and reliable. This setup allows multiple producers and consumers to interact with the data seamlessly, even though I only used one for now.

### Consumers

For this project, I selected Apache Spark for sentiment analysis, as it allows for the use of various machine learning models to analyze the comments efficiently. Once the sentiment analysis is performed, the results are pushed into a DataFrame, so that further statistical analysis can be performed, making it easier to manipulate and query the data. I used DataBricks and Google Colab for this step, as both of which support Jupyter Notebook files (`.ipynb`), to run the Spark jobs and present the analysis results in an accessible manner.

## End Result

Overall, I was very happy with the end result of this project. Through this, I managed to effectively gauge the overall reaction to my content, and learnt more about data engineering and databases in general. 

Here are some examples of the analyzer in action. This analysis was performed on my own comments
![Analysis performed on comments on my own videos](mycomments.png)

This second analysis was performed on a recent hockey sports highlights video, showcasing the real-time capabilities of the system.
![Analysis performed on recent sports comments](sportscomments.png)

## Next Steps

I appreciate the insights provided by Spark's analysis, and I recognize that there's always room for growth. I'm eager to explore ways to better understand viewer sentiment and extract deeper insights from comments, especially those that contribute valuable context. I'm open to any suggestions or resources that could help me enhance this aspect of my work. My goal is to leverage all available tools to create content that resonates more effectively with my audience.

![Analysis performed on top comments of my own videos](mycomments2.png)

Ultimately, I wish to extract even more nuanced insights from viewer comments, so that I can further align my content with their preferences and  enhance my ability to engage and connect with my audience, as where would I be without them?

Thanks for reading!
