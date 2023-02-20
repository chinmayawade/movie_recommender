<p>&nbsp;</p>
# Movie recommender System using Machine Learning with Model Deployment on Web using Heroku #
Recommender System is a system that seeks to predict or filter preferences according to the user’s choices. Recommender systems are utilized in a variety of areas including movies, music, news, books, research articles, search queries, social tags, and products in general. <br>
Recommender systems produce a list of recommendations in any of the two ways – Collabrative Filtering and Content-based Filtering
<p>&nbsp;</p>
## Collabrative Filtering and Content-based Filtering ##

Collabrative Filtering| Content-based Filtering
--------------------- | -----------------------
Collaborative filtering is a technique used by recommender systems.| Content-based filtering uses item features to recommend other items similar to what the user likes, based on their previous actions or explicit feedback.
In Collaborative Filtering, we tend to find similar users and recommend what similar users like.| A Content-Based Recommender works by the data that we take from the user, either explicitly (rating) or implicitly (clicking on a link).
In this type of recommendation system, we don’t use the features of the item to recommend it, rather we classify the users into the clusters of similar types, and recommend each user according to the preference of its cluster.|By the data we create a user profile, which is then used to suggest to the user, as the user provides more input or take more actions on the recommendation, the engine becomes more accurate.


## Collecting Datasets from API's | [Visit TMBD API Documentation](https://developers.themoviedb.org/3)

### What is TMDB's API? ###
***********************************************
The API service is for those of you interested in using our movie, TV show or actor images and/or data in your application. Our API is a system we provide for you and your team to programmatically fetch and use our data and/or images.



### General Features ###
*************************************************
* Top rated movies
* Upcoming movies
* Now playing movies
* Popular movies
* Popular TV shows
* Top rated TV shows
* On the air TV shows
* Airing today TV shows
* Popular people

<p>&nbsp;</p>
## Importing Datasets ##
### 1.  Movie Dataset###
*****
![Alt text](https://res.cloudinary.com/dntkxtebe/image/upload/q_1000/v1663917568/Screenshot_96_ba7mk1.png)
<p>&nbsp;</p>
### 2. Credits Dataset ###
*****
![Alt text](https://res.cloudinary.com/dntkxtebe/image/upload/q_1000000/v1663920667/Screenshot_97_adg9aa.png)
<p>&nbsp;</p>


## Python Pickle Model | Go to [Python Pickle Documentation](https://docs.python.org/3/library/pickle.html).
*************************************************************
Python pickle module is used for serializing and de-serializing a Python object structure. Any object in Python can be pickled so that it can be saved on disk. What pickle does is that it “serializes” the object first before writing it to file. Pickling is a way to convert a python object (list, dict, etc.) into a character stream. The idea is that this character stream contains all the information necessary to reconstruct the object in another python script.

## Streamlit Python | Go to [streamlit.io](https://streamlit.io/).

*****************************************************
Streamlit is an open-source Python library that makes it easy to create and share beautiful, custom web apps for machine learning and data science. In just a few minutes you can build and deploy powerful data apps.

## Heroku App |  Visit [Heroku](https://id.heroku.com/login)
*********************************************
Heroku is a container-based cloud Platform as a Service (PaaS). Developers use Heroku to deploy, manage, and scale modern apps. Our platform is elegant, flexible, and easy to use, offering developers the simplest path to getting their apps to market.


## Setup SSH Client .ssh file ##
********
```
mkdir -p ~/.streamlit/
echo "\
[server]\n\
headless = true\n\
port = $PORT\n\
enableCORS = false\n\
\n\
" > ~/.streamlit/config.toml
```

## Procfile.txt ##
A Procfile is a mechanism for declaring what commands are run by your application's containers on the Deis platform. It follows the process model. You can use a Procfile to declare various process types, such as multiple types of workers, a singleton process like a clock, or a consumer of the Twitter streaming API.
****

```
web: gunicorn app:app sh setup.sh && streamlit run app.py
```

## Creating the app.py file to run the project ##
*****
```
import streamlit as st
import pickle
import pandas as pd

def recommend(movie):
    movie_index = movies[movies['title'] == movie].index[0] 
    distances = similarity[movie_index]
    movie_list = sorted(list(enumerate( distances)),reverse =True , key = lambda x:x[1])[1:6]
    
    recommended_movies = []
    
    for i in movie_list:
        recommended_movies.append(movies.iloc[i[0]].title)
    return recommended_movies
        
movies_dict = pickle.load(open('movies_dict.pkl','rb'))
movies = pd.DataFrame(movies_dict)

similarity = pickle.load(open('similarity.pkl','rb'))

st.title('Movie Recommender System')

option = st.selectbox('Slect your Movie:', movies['title'].values)

if st.button('Recommend'):
    recommendations = recommend(option)
    for i in recommendations:
        st.write(i)

```

