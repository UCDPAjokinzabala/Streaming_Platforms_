Scanning# Libraries to be used 

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt


# Source data (defining a different codec package) 

description=pd.read_csv(r'\\sedna\Jokin.Ormazabal$\$Profile\Desktop\UCD\TopPlatforms.csv',encoding= 'unicode_escape')
netflix=pd.read_csv(r'\\sedna\Jokin.Ormazabal$\$Profile\Desktop\UCD\netflix_titles.csv',encoding= 'unicode_escape')
amazon=pd.read_csv(r'\\sedna\Jokin.Ormazabal$\$Profile\Desktop\UCD\amazon_prime_titles.csv',encoding= 'unicode_escape')
disney=pd.read_csv(r'\\sedna\Jokin.Ormazabal$\$Profile\Desktop\UCD\disney_plus_titles.csv',encoding= 'unicode_escape')


# Get the information for each table and clean the data (drop the colummns/rows that we dont need)

print('------------ TOP PLATFORMS ------------')

description=description.drop(labels=[0],axis=0)
description=description.drop(labels=range(4,16),axis=0)
description = description.set_index("Name") #use the name as index

print(np.shape(description))
print(description.info())


print('------------ NETFLIX ------------')

print(description.iloc[0,0]) # give context to the data
netflix=netflix.drop(['director','cast','description'],axis=1)

print(np.shape(netflix))
print(netflix.info())

# Count the unique values in each column to get the primary key
title_count = pd.unique(netflix.title)
title_unique=0

for x in title_count:
    title_unique +=1


print(f'Count of title_id : {title_unique}') #title is the primary key 
     
     
     # Check for duplicates, null and missing values

#Duplicates (we know that we will have duplicates on some columns by the nature of these, but we check for duplicates on the entire set)
drop_duplicates= netflix.drop_duplicates()
print('The Netflix dataset has NO duplicated values') if netflix.shape == drop_duplicates.shape else print('The Netflix dataset has duplicated values')

#NA VALUES
na_values_netflix = netflix.isna().sum()
na_netflix = na_values_netflix.to_numpy()
print('The Netflix dataset has NO na values') if np.sum(na_netflix) == 0 else print('The Netlifx dataset has na values')

#There are null values so we replace them for 0 and check again
cleaned_netflix = netflix.fillna(0) 
na_values_netflix = cleaned_netflix.isna().sum()
na_netflix = na_values_netflix.to_numpy()
print('The cleaned_Netflix dataset has NO na values') if np.sum(na_netflix) == 0 else print('The cleaned_Netlifx dataset has na values')

netflix=cleaned_netflix #remap the netflix data with the cleaned dataframe 

#Missing values
missing_values_netflix = netflix.isnull().sum().values
print('The netflix dataset has NO null values') if np.sum(missing_values_netflix) == 0 else print('The netflix dataset has null values')

#Delete un-used variables 
del cleaned_netflix,drop_duplicates,missing_values_netflix,na_values_netflix,na_netflix,title_count,title_unique,x 

print('------------ NETFLIX DATA HAS BEEN CHECKED AND CLEANED ------------')

print('------------ AMAZON PRIME VIDEO ------------') #we repeat the same for amazon (data has the same logic than netflix)

print(description.iloc[1,0]) # give context to the data
amazon=amazon.drop(['director','cast','description'],axis=1)

print(np.shape(amazon))
print(amazon.info())

# Count the unique values in each column to get the primary key

print(len(amazon.title.unique())) # different way of checking the primary key
     
     
     # Check for duplicates, null and missing values

#Duplicates (we know that we will have duplicates on some columns by the nature of these, but we check for duplicates on the entire set)
drop_duplicates= amazon.drop_duplicates()
print('The Amazon dataset has NO duplicated values') if amazon.shape == drop_duplicates.shape else print('The Amazon dataset has duplicated values')

#NA VALUES
na_values_amazon = amazon.isna().sum()
na_amazon = na_values_amazon.to_numpy()
print('The Amazon dataset has NO na values') if np.sum(na_amazon) == 0 else print('The Amazon dataset has na values')

#There are null values so we replace them for 0 and check again
cleaned_amazon = amazon.fillna(0) 
na_values_amazon= cleaned_amazon.isna().sum()
na_amazon = na_values_amazon.to_numpy()
print('The cleaned_Amazon dataset has NO na values') if np.sum(na_amazon) == 0 else print('The cleaned_Amazon dataset has na values')

amazon=cleaned_amazon #remap the Amazon data with the cleaned dataframe 

#Missing values
missing_values_amazon = amazon.isnull().sum().values
print('The Amazon dataset has NO null values') if np.sum(missing_values_amazon) == 0 else print('The Amazon dataset has null values')

#Delete un-used variables 
del cleaned_amazon,drop_duplicates,missing_values_amazon,na_values_amazon,na_amazon

print('------------ AMAZON DATA HAS BEEN CHECKED AND CLEANED ------------')

print('------------ DISNEY + ------------') #we repeat the same exercise for the last time 

print(description.iloc[2,0]) # give context to the data
disney=disney.drop(['director','cast','description'],axis=1)

print(np.shape(disney))
print(disney.info())

# Count the unique values in each column to get the primary key

print(len(disney.title.unique())) 
     
     
     # Check for duplicates, null and missing values

#Duplicates (we know that we will have duplicates on some columns by the nature of these, but we check for duplicates on the entire set)
drop_duplicates= disney.drop_duplicates()
print('The Disney dataset has NO duplicated values') if disney.shape == drop_duplicates.shape else print('The Dinesy dataset has duplicated values')

#NA VALUES
na_values_disney = disney.isna().sum()
na_disney = na_values_disney.to_numpy()
print('The Disney dataset has NO na values') if np.sum(na_disney) == 0 else print('The Disney dataset has na values')

#There are null values so we replace them for 0 and check again
cleaned_disney = disney.fillna(0) 
na_values_disney= cleaned_disney.isna().sum()
na_disney= na_values_disney.to_numpy()
print('The cleaned_Disney dataset has NO na values') if np.sum(na_disney) == 0 else print('The cleaned_Disney dataset has na values')

disney=cleaned_disney #remap the Amazon data with the cleaned dataframe 

#Missing values
missing_values_disney = disney.isnull().sum().values
print('The Disney dataset has NO null values') if np.sum(missing_values_disney) == 0 else print('The Disney dataset has null values')

#Delete un-used variables 
del cleaned_disney,drop_duplicates,missing_values_disney,na_values_disney,na_disney

print('------------ DISNEY DATA HAS BEEN CHECKED AND CLEANED ------------')

print('------------ ALL DATA IS READY FOR ANALYSIS  ------------')

# we identify the common titles between the platforms to analize their relationship
    
    #NETFLIX AND AMAZON

netflix_amazon= netflix.merge(amazon, on= 'title', how = 'inner')
netflix_amazon=netflix_amazon.loc[:, netflix_amazon.columns.intersection(['title'])]
net_ama=(len(netflix_amazon.title.unique())) 
print("there are",net_ama,"common titles on Netflix and Amazon")
    
    #NETFLIX AND DISNEY

netflix_disney=netflix.merge(disney, on= 'title', how = 'inner')
netflix_disney=netflix_disney.loc[:, netflix_disney.columns.intersection(['title'])]
net_dis=(len(netflix_disney.title.unique())) 
print("there are",net_dis,"common titles on Netflix and Disney")
    
    #AMAZON AND DISNEY

amazon_disney=amazon.merge(disney, on= 'title', how = 'inner')
amazon_disney=amazon_disney.loc[:, amazon_disney.columns.intersection(['title'])]
ama_dis=(len(amazon_disney.title.unique())) 
print("there are",ama_dis,"common titles on Netflix and Disney")
    
    #AMAZON,NETFLIX AND DISNEY

netflix_amazon_disney=netflix.merge(amazon,on='title').merge(disney,on='title')
netflix_amazon_disney=netflix_amazon_disney.loc[:, netflix_amazon_disney.columns.intersection(['title'])]
net_ama_dis=(len(netflix_amazon_disney.title.unique())) 
print("there is",net_ama_dis,"common title in Amazon,Netflix and Disney")

#relationships (number of common data / total)

count_netflix=(len(netflix.title.unique())) 
count_amazon=(len(amazon.title.unique()))
count_disney=(len(disney.title.unique()))

count_netflix_amazon=count_netflix+count_amazon
count_netflix_disney=count_netflix+count_disney
count_amazon_disney=count_amazon+count_disney
count_amazon_disney_netflix=count_amazon+count_disney+count_netflix

relationship_netflix_amazon=((net_ama*100.0)/count_netflix_amazon)
relationship_netflix_disney=((net_dis*100.0)/count_netflix_disney)
relationship_amazon_disney=((ama_dis*100.0)/count_amazon_disney)
relationship_amazon_disney_netflix=((net_ama_dis*100.0)/count_amazon_disney_netflix)


relationship = pd.DataFrame({"Platforms":["Common_titles","Total_Titles","% of Common Titles"],
                    "Netflix and Amazon":[net_ama,count_netflix_amazon,relationship_netflix_amazon],           # Create pandas DataFrame
                     "Netflix and Disney":[net_dis,count_netflix_disney,relationship_netflix_disney],
                     "Amazon and Disney":[ama_dis,count_amazon_disney,relationship_amazon_disney],
                    "Amazon,Netflix and Disney":[net_ama_dis,count_amazon_disney_netflix,relationship_amazon_disney_netflix]})
    
relationship = relationship.set_index("Platforms")


#clean the variable explorer (both for memory and to focus on the important)


del ama_dis,amazon_disney,count_amazon_disney,count_amazon_disney_netflix,count_netflix_disney,net_ama,count_netflix_amazon,net_ama_dis,net_dis,netflix_amazon,netflix_amazon_disney,netflix_disney,relationship_amazon_disney,relationship_amazon_disney_netflix,relationship_netflix_amazon,relationship_netflix_disney


#Exploratory data analysis of NETFLIX

print('------------ NETFLIX EXPLORATORY DATA ANALYSIS ------------')

print('------------ NETFLIX TITLES BY GENRE ------------')
    #Titles by genre

netflix_genre=list(netflix['listed_in'])

empty=[]

for x in netflix_genre:
    temp=x.split(', ')
    for y in temp:
        empty.append(y)

empty=pd.DataFrame(empty)
empty.columns=['genre']
group=empty.groupby('genre').agg({'genre':'count'})
titles_genre_netflix=group.apply(lambda x: x.sort_values(ascending=False))
titles_genre_netflix = titles_genre_netflix.head(10)
print(titles_genre_netflix.head(10))

plt.title("NETFLIX TITLES BY GENRE")
plt.barh(titles_genre_netflix.index,titles_genre_netflix.genre)
plt.show()

del empty,group,netflix_genre,x,y,temp,titles_genre_netflix

    #By type of shows

print('------------PERCENTAGE OF TYPE OF SHOWS ------------')

netflix_by_type=netflix.groupby('type').agg({'type':'count'})

count_netflix_movie=netflix.type.str.contains(r'Movie').sum() +0.0 #count how many movies does Netflix have
count_netflix_tvshows=netflix.type.str.contains(r'TV Show').sum() +0.0 

percentage_movie=(count_netflix_movie*100)/count_netflix
percentage_TvShow=(count_netflix_tvshows*100)/count_netflix

percentage_movie_label='Movies: ' + str(round(percentage_movie))+'%'
percentage_tvshows_label='TV Shows: ' + str(round(percentage_TvShow))+'%'

percentage=[percentage_movie,percentage_TvShow]
netflix_by_type['Percentage']=percentage

slices=[percentage_movie,percentage_TvShow]
lables=[percentage_movie_label,percentage_tvshows_label]
print(netflix_by_type.head())

plt.title("NETFLIX TITLES BY TYPE OF SHOWS")
plt.pie(slices,labels=lables,wedgeprops={'edgecolor':'black'}) #Generate a piechart with a black edge 
plt.show()


del lables,netflix_by_type,percentage,percentage_TvShow,percentage_movie,percentage_movie_label,percentage_tvshows_label,slices,count_netflix_movie,count_netflix_tvshows


print('------------ NETFLIX TITLES BY COUNTRY OF PRODUCTION ------------')
    #Titles by genre

netflix_country=netflix[netflix.country != 0]
netflix_country=list(netflix_country['country'])

empty=[]


for x in netflix_country:
    temp=x.split(', ')
    for y in temp:
        empty.append(y)

empty=pd.DataFrame(empty)
empty.columns=['country']
group=empty.groupby('country').agg({'country':'count'})
titles_country_netflix=group.apply(lambda x: x.sort_values(ascending=False))
titles_country_netflix = titles_country_netflix.head(10)
print(titles_country_netflix.head(10))

plt.title("NETFLIX TITLES BY COUNTRY OF PRODUCTION")
plt.barh(titles_country_netflix.index,titles_country_netflix.country)
plt.show()

del empty,group,netflix_country,x,y,temp,titles_country_netflix


print('------------NETFLIX TITLES BY RATING ------------')

netflix_rating=list(netflix['rating'])

empty=[]


#for x in netflix_rating:
    #temp=x.split(', ')
for y in netflix_rating:
    empty.append(y)

empty=pd.DataFrame(empty)
empty.columns=['rating']
group=empty.groupby('rating').agg({'rating':'count'})
titles_rating_netflix=group.apply(lambda x: x.sort_values(ascending=False))
titles_rating_netflix = titles_rating_netflix.head(9)
print(titles_rating_netflix.head(9))

plt.title("NETFLIX TITLES BY RATING")
plt.bar(titles_rating_netflix.index,titles_rating_netflix.rating)
plt.show()

#Get the median of duration and dates

# By release date 
print('------------FINAL INSIGHTS ------------')

netflix['date_added_year']=pd.DatetimeIndex(netflix['date_added']).year
netflix['month']=pd.DatetimeIndex(netflix['date_added']).month
netflix_release_year=netflix.groupby(['date_added_year','month']).agg({'month':'count'}).unstack()

netflix['diff_year']=netflix['date_added_year']-netflix['release_year']

netflix_modernity_mean=netflix["diff_year"].mean()

round(netflix_modernity_mean)

print("Netflix, by average shows titles",round(netflix_modernity_mean),"years after their release date")


netflix_movie=netflix[netflix.type != 'TV Show'] #we exclude tv_shows to get the average duration of movies
netflix_movie = netflix_movie['duration'].str.replace(' min','') #replace the word min for nothing to convert it to float
netflix_movie = netflix_movie.to_frame() # convert the series into data frame 
netflix_movie=netflix_movie['duration'].astype(float) 
netflix_movie = netflix_movie.to_frame() # convert again the series into data frame 
netflix_duration_movies=netflix_movie["duration"].mean() # mean calculation

round(netflix_duration_movies)

print("Netflix, by average shows movies with",round(netflix_duration_movies),"minutes")


#we exclude Movies to get the average duration of tv shows

netflix_tv=netflix[netflix.type != 'Movie']
netflix_tv= netflix_tv['duration'].str.replace(' Seasons','',)
netflix_tv = netflix_tv.to_frame()
netflix_tv = netflix_tv['duration'].str.replace(' Season','')
netflix_tv = netflix_tv.to_frame()
netflix_tv=netflix_tv['duration'].astype(float)
netflix_tv = netflix_tv.to_frame()
netflix_duration_tv=netflix_tv["duration"].mean()

round(netflix_duration_tv)

print("Netflix, by average shows TV Shows with",round(netflix_duration_tv),"seasons ")

del count_netflix,empty,group,netflix_duration_movies,netflix_duration_tv,netflix_modernity_mean,netflix_movie,netflix_rating,netflix_release_year,netflix_tv,titles_rating_netflix,y


print('------------ AMAZON EXPLORATORY DATA ANALYSIS ------------') #REPLICATE SAME MODEL FOR AMAZON
print('------------ AMAZON TITLES BY GENRE ------------')
    #Titles by genre

amazon_genre=list(amazon['listed_in'])

empty=[]


for x in amazon_genre:
    temp=x.split(', ')
    for y in temp:
        empty.append(y)

empty=pd.DataFrame(empty)
empty.columns=['genre']
group=empty.groupby('genre').agg({'genre':'count'})
titles_genre_amazon=group.apply(lambda x: x.sort_values(ascending=False))
titles_genre_amazon = titles_genre_amazon.head(10)
print(titles_genre_amazon.head(10))

plt.title("AMAZON TITLES BY GENRE")
plt.barh(titles_genre_amazon.index,titles_genre_amazon.genre)
plt.show()

del empty,group,amazon_genre,x,y,temp,titles_genre_amazon

print('------------PERCENTAGE OF TYPE OF SHOWS ------------')

amazon_by_type=amazon.groupby('type').agg({'type':'count'})

count_amazon_movie=amazon.type.str.contains(r'Movie').sum() +0.0 #count how many movies does Netflix have
count_amazon_tvshows=netflix.type.str.contains(r'TV Show').sum() +0.0 

percentage_movie=(count_amazon_movie*100)/count_amazon
percentage_TvShow=(count_amazon_tvshows*100)/count_amazon

percentage_movie_label='Movies: ' + str(round(percentage_movie))+'%'
percentage_tvshows_label='TV Shows: ' + str(round(percentage_TvShow))+'%'

percentage=[percentage_movie,percentage_TvShow]
amazon_by_type['Percentage']=percentage

slices=[percentage_movie,percentage_TvShow]
lables=[percentage_movie_label,percentage_tvshows_label]
print(amazon_by_type.head())

plt.title("AMAZON TITLES BY TYPE OF SHOWS")
plt.pie(slices,labels=lables,wedgeprops={'edgecolor':'black'}) #Generate a piechart with a black edge 
plt.show()


del lables,amazon_by_type,percentage,percentage_TvShow,percentage_movie,percentage_movie_label,percentage_tvshows_label,slices,count_amazon_movie,count_amazon_tvshows

print('------------ AMAZON TITLES BY COUNTRY OF PRODUCTION ------------')
    #Titles by genre

amazon_country=amazon[amazon.country != 0]
amazon_country=list(amazon_country['country'])

empty=[]


for x in amazon_country:
    temp=x.split(', ')
    for y in temp:
        empty.append(y)

empty=pd.DataFrame(empty)
empty.columns=['country']
group=empty.groupby('country').agg({'country':'count'})
titles_country_amazon=group.apply(lambda x: x.sort_values(ascending=False))
titles_country_amazon = titles_country_amazon.head(10)
print(titles_country_amazon.head(10))

plt.title("AMAZON TITLES BY COUNTRY OF PRODUCTION")
plt.barh(titles_country_amazon.index,titles_country_amazon.country)
plt.show()

del empty,group,amazon_country,x,y,temp,titles_country_amazon

print('------------AMAZON TITLES BY RATING ------------')

amazon_rating = amazon.fillna(0) 
amazon_rating=amazon_rating[amazon_rating.rating != 0]
amazon_rating=list(amazon_rating['rating'])

empty=[]

for y in amazon_rating:
    empty.append(y)

empty=pd.DataFrame(empty)
empty.columns=['rating']
group=empty.groupby('rating').agg({'rating':'count'})
titles_rating_amazon=group.apply(lambda x: x.sort_values(ascending=False))
titles_rating_amazon = titles_rating_amazon.head(9)
print(titles_rating_amazon.head(9))

plt.title("AMAZON TITLES BY RATING")
plt.bar(titles_rating_amazon.index,titles_rating_amazon.rating)
plt.show()

#Get the median of duration and dates

# By release date 
print('------------FINAL INSIGHTS ------------')

amazon= amazon.fillna(0) 
amazon=amazon[amazon.date_added != 0]
amazon['date_added_year']=pd.DatetimeIndex(amazon['date_added']).year
amazon['month']=pd.DatetimeIndex(amazon['date_added']).month
amazon_release_year=amazon.groupby(['date_added_year','month']).agg({'month':'count'}).unstack()

amazon['diff_year']=amazon['date_added_year']-amazon['release_year']

amazon_modernity_mean=amazon["diff_year"].mean()

round(amazon_modernity_mean)

print("Amazon, by average shows titles",round(amazon_modernity_mean),"years after their release date")


amazon_movie=amazon[amazon.type != 'TV Show'] #we exclude tv_shows to get the average duration of movies
amazon_movie = amazon_movie['duration'].str.replace(' min','') #replace the word min for nothing to convert it to float
amazon_movie = amazon_movie.to_frame() # convert the series into data frame 
amazon_movie=amazon_movie['duration'].astype(float) 
amazon_movie = amazon_movie.to_frame() # convert again the series into data frame 
amazon_duration_movies=amazon_movie["duration"].mean() # mean calculation

round(amazon_duration_movies)

print("Amazon, by average shows movies with",round(amazon_duration_movies),"minutes")


#we exclude Movies to get the average duration of tv shows

amazon_tv=amazon[amazon.type != 'Movie']
amazon_tv= amazon_tv['duration'].str.replace(' Seasons','',)
amazon_tv = amazon_tv.to_frame()
amazon_tv = amazon_tv['duration'].str.replace(' Season','')
amazon_tv = amazon_tv.to_frame()
amazon_tv=amazon_tv['duration'].astype(float)
amazon_tv = amazon_tv.to_frame()
amazon_duration_tv=amazon_tv["duration"].mean()

round(amazon_duration_tv)

print("Amazon,by average shows TV Shows with",round(amazon_duration_tv),"seasons ")

del count_amazon,empty,group,amazon_duration_movies,amazon_duration_tv,amazon_modernity_mean,amazon_movie,amazon_rating,amazon_release_year,amazon_tv,titles_rating_amazon,y

print('------------ DISNEY + EXPLORATORY DATA ANALYSIS ------------') #REPLICATE SAME MODEL FOR AMAZON
print('------------ DISNEY + TITLES BY GENRE ------------')
    #Titles by genre

disney_genre=list(disney['listed_in'])

empty=[]


for x in disney_genre:
    temp=x.split(', ')
    for y in temp:
        empty.append(y)

empty=pd.DataFrame(empty)
empty.columns=['genre']
group=empty.groupby('genre').agg({'genre':'count'})
titles_genre_disney=group.apply(lambda x: x.sort_values(ascending=False))
titles_genre_disney = titles_genre_disney.head(10)
print(titles_genre_disney.head(10))

plt.title("DISNEY + TITLES BY GENRE")
plt.barh(titles_genre_disney.index,titles_genre_disney.genre)
plt.show()

del empty,group,disney_genre,x,y,temp,titles_genre_disney

print('------------PERCENTAGE OF TYPE OF SHOWS ------------')

disney_by_type=disney.groupby('type').agg({'type':'count'})

count_disney_movie=disney.type.str.contains(r'Movie').sum() +0.0 #count how many movies does Netflix have
count_disney_tvshows=disney.type.str.contains(r'TV Show').sum() +0.0 

percentage_movie=(count_disney_movie*100)/count_disney
percentage_TvShow=(count_disney_tvshows*100)/count_disney

percentage_movie_label='Movies: ' + str(round(percentage_movie))+'%'
percentage_tvshows_label='TV Shows: ' + str(round(percentage_TvShow))+'%'

percentage=[percentage_movie,percentage_TvShow]
disney_by_type['Percentage']=percentage

slices=[percentage_movie,percentage_TvShow]
lables=[percentage_movie_label,percentage_tvshows_label]
print(disney_by_type.head())

plt.title("DISNEY + TITLES BY TYPE OF SHOWS")
plt.pie(slices,labels=lables,wedgeprops={'edgecolor':'black'}) #Generate a piechart with a black edge 
plt.show()


del lables,disney_by_type,percentage,percentage_TvShow,percentage_movie,percentage_movie_label,percentage_tvshows_label,slices,count_disney_movie,count_disney_tvshows

print('------------ DISNEY + TITLES BY COUNTRY OF PRODUCTION ------------')
    #Titles by genre

disney_country=disney[disney.country != 0]
disney_country=list(disney_country['country'])

empty=[]


for x in disney_country:
    temp=x.split(', ')
    for y in temp:
        empty.append(y)

empty=pd.DataFrame(empty)
empty.columns=['country']
group=empty.groupby('country').agg({'country':'count'})
titles_country_disney=group.apply(lambda x: x.sort_values(ascending=False))
titles_country_disney = titles_country_disney.head(10)
print(titles_country_disney.head(10))

plt.title("DISNEY + TITLES BY COUNTRY OF PRODUCTION")
plt.barh(titles_country_disney.index,titles_country_disney.country)
plt.show()

del empty,group,disney_country,x,y,temp,titles_country_disney

print('------------DISNEY + TITLES BY RATING ------------')

disney_rating = disney.fillna(0) 
disney_rating=disney_rating[disney_rating.rating != 0]
disney_rating=list(disney_rating['rating'])

empty=[]

for y in disney_rating:
    empty.append(y)

empty=pd.DataFrame(empty)
empty.columns=['rating']
group=empty.groupby('rating').agg({'rating':'count'})
titles_rating_disney=group.apply(lambda x: x.sort_values(ascending=False))
titles_rating_disney = titles_rating_disney.head(9)
print(titles_rating_disney.head(9))

plt.title("DISNEY + TITLES BY RATING")
plt.bar(titles_rating_disney.index,titles_rating_disney.rating)
plt.show()

#Get the median of duration and dates

# By release date 
print('------------FINAL INSIGHTS ------------')

disney= disney.fillna(0) 
disney=disney[disney.date_added != 0]
disney['date_added_year']=pd.DatetimeIndex(disney['date_added']).year
disney['month']=pd.DatetimeIndex(disney['date_added']).month
disney_release_year=disney.groupby(['date_added_year','month']).agg({'month':'count'}).unstack()

disney['diff_year']=disney['date_added_year']-disney['release_year']

disney_modernity_mean=disney["diff_year"].mean()

round(disney_modernity_mean)

print("Disney +, by average shows titles",round(disney_modernity_mean),"years after their release date")


disney_movie=disney[disney.type != 'TV Show'] #we exclude tv_shows to get the average duration of movies
disney_movie = disney_movie['duration'].str.replace(' min','') #replace the word min for nothing to convert it to float
disney_movie = disney_movie.to_frame() # convert the series into data frame 
disney_movie=disney_movie['duration'].astype(float) 
disney_movie = disney_movie.to_frame() # convert again the series into data frame 
disney_duration_movies=disney_movie["duration"].mean() # mean calculation

round(disney_duration_movies)

print("Disney +, by average shows movies with",round(disney_duration_movies),"minutes")


#we exclude Movies to get the average duration of tv shows

disney_tv=disney[disney.type != 'Movie']
disney_tv= disney_tv['duration'].str.replace(' Seasons','',)
disney_tv = disney_tv.to_frame()
disney_tv = disney_tv['duration'].str.replace(' Season','')
disney_tv = disney_tv.to_frame()
disney_tv=disney_tv['duration'].astype(float)
disney_tv = disney_tv.to_frame()
disney_duration_tv=disney_tv["duration"].mean()

round(disney_duration_tv)

print("Disney +,by average shows TV Shows with",round(disney_duration_tv),"seasons ")

del count_disney,empty,group,disney_duration_movies,disney_duration_tv,disney_modernity_mean,disney_movie,disney_rating,disney_release_year,disney_tv,titles_rating_disney,y







