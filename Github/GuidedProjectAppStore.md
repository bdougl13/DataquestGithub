
# Douglas Data Analyis Guided Project

### About the Project
This project is about analyzing data for both Android and iOS mobile apps available on both Apple Store and Google Play. The pseudo-company builds free apps where the main source of revenue is from in-app ads. The goal is to analyze the data to help developers understand what type of apps can attract more users.

Add link to documentation*
###### Supported by DataQuest



```python
opened_file = open('AppleStore.csv')
from csv import reader
read_file = reader(opened_file)
apple_apps_data = list(read_file)

opened_file = open('googleplaystore.csv')
from csv import reader
read_file = reader(opened_file)
android_apps_data = list(read_file)

```


```python
def explore_data(dataset, start, end, rows_and_columns=False):
    dataset_slice = dataset[start:end]    
    for row in dataset_slice:
        print(row)
        print('\n') # adds a new (empty) line after each row

    if rows_and_columns:
        print('Number of rows:', len(dataset))
        print('Number of columns:', len(dataset[0]))
```


```python
print(apple_apps_data[0])
print('\n')
print(explore_data(apple_apps_data, 1,4, True))

print(android_apps_data[0])
print('\n')
print(explore_data(android_apps_data, 1,4, True))

```

    ['id', 'track_name', 'size_bytes', 'currency', 'price', 'rating_count_tot', 'rating_count_ver', 'user_rating', 'user_rating_ver', 'ver', 'cont_rating', 'prime_genre', 'sup_devices.num', 'ipadSc_urls.num', 'lang.num', 'vpp_lic']
    
    
    ['284882215', 'Facebook', '389879808', 'USD', '0.0', '2974676', '212', '3.5', '3.5', '95.0', '4+', 'Social Networking', '37', '1', '29', '1']
    
    
    ['389801252', 'Instagram', '113954816', 'USD', '0.0', '2161558', '1289', '4.5', '4.0', '10.23', '12+', 'Photo & Video', '37', '0', '29', '1']
    
    
    ['529479190', 'Clash of Clans', '116476928', 'USD', '0.0', '2130805', '579', '4.5', '4.5', '9.24.12', '9+', 'Games', '38', '5', '18', '1']
    
    
    Number of rows: 7198
    Number of columns: 16
    None
    ['App', 'Category', 'Rating', 'Reviews', 'Size', 'Installs', 'Type', 'Price', 'Content Rating', 'Genres', 'Last Updated', 'Current Ver', 'Android Ver']
    
    
    ['Photo Editor & Candy Camera & Grid & ScrapBook', 'ART_AND_DESIGN', '4.1', '159', '19M', '10,000+', 'Free', '0', 'Everyone', 'Art & Design', 'January 7, 2018', '1.0.0', '4.0.3 and up']
    
    
    ['Coloring book moana', 'ART_AND_DESIGN', '3.9', '967', '14M', '500,000+', 'Free', '0', 'Everyone', 'Art & Design;Pretend Play', 'January 15, 2018', '2.0.0', '4.0.3 and up']
    
    
    ['U Launcher Lite – FREE Live Cool Themes, Hide Apps', 'ART_AND_DESIGN', '4.7', '87510', '8.7M', '5,000,000+', 'Free', '0', 'Everyone', 'Art & Design', 'August 1, 2018', '1.2.4', '4.0.3 and up']
    
    
    Number of rows: 10842
    Number of columns: 13
    None



```python
##Checking data for duplicates or errors via discussion sections
print(android_apps_data[10473])
del android_apps_data[10473]
```

    ['Life Made WI-Fi Touchscreen Photo Frame', '1.9', '19', '3.0M', '1,000+', 'Free', '0', 'Everyone', '', 'February 11, 2018', '1.0.19', '4.0 and up']


There are some apps that repeat and appear as duplicates in the dataset. For example, Instagram appears more than once.


```python
for app in android_apps_data:
    name = app[0]
    if name == 'Instagram':
        print(app)
```

    ['Instagram', 'SOCIAL', '4.5', '66577313', 'Varies with device', '1,000,000,000+', 'Free', '0', 'Teen', 'Social', 'July 31, 2018', 'Varies with device', 'Varies with device']
    ['Instagram', 'SOCIAL', '4.5', '66577446', 'Varies with device', '1,000,000,000+', 'Free', '0', 'Teen', 'Social', 'July 31, 2018', 'Varies with device', 'Varies with device']
    ['Instagram', 'SOCIAL', '4.5', '66577313', 'Varies with device', '1,000,000,000+', 'Free', '0', 'Teen', 'Social', 'July 31, 2018', 'Varies with device', 'Varies with device']
    ['Instagram', 'SOCIAL', '4.5', '66509917', 'Varies with device', '1,000,000,000+', 'Free', '0', 'Teen', 'Social', 'July 31, 2018', 'Varies with device', 'Varies with device']



```python
duplicate_apps = []
unique_apps = []

for app in android_apps_data:
    name = app[0]
    if name in unique_apps:
        duplicate_apps.append(name)
    else:
        unique_apps.append(name)
        
print('Number of duplicate apps:', len(duplicate_apps))
print('\n')
print('Examples of duplicate apps:', duplicate_apps[:15])
```

    Number of duplicate apps: 1181
    
    
    Examples of duplicate apps: ['Quick PDF Scanner + OCR FREE', 'Box', 'Google My Business', 'ZOOM Cloud Meetings', 'join.me - Simple Meetings', 'Box', 'Zenefits', 'Google Ads', 'Google My Business', 'Slack', 'FreshBooks Classic', 'Insightly CRM', 'QuickBooks Accounting: Invoicing & Expenses', 'HipChat - Chat Built for Teams', 'Xero Accounting Software']


This has to deal with the change in reviews being updated. These duplicate apps won't randomly be removed. To clean the data, the rows with the least amount of reviews (which correspond to less recent data) need to be removed.


```python
reviews_max = {}
for app in android_apps_data[1:]:
    name = app[0]
    n_reviews = float(app[3])
    
    if name in reviews_max and reviews_max[name] < n_reviews:
        reviews_max[name] = n_reviews
    elif name not in reviews_max:
        reviews_max[name] = n_reviews
print('actual length:', len(reviews_max))
print('expected length:', len(android_apps_data[1:]) - 1181)
```

    actual length: 9659
    expected length: 9659


In the previous above step I created a dictionary for the unique apps with the highest number of reviews, for that is the most recent data on the app, to filter out the duplictates. The length of the list should be the same is the length of the android_apps_data without the duplicates, which they do.
To do this I had to:
- created an empty dictionary
- convert the number of reviews to a float
- loop over all the apps and pick the one with the highest reviews for duplicate apps
- also include the unique apps inside the dictionary

Because I included the header row in my code I have to take that into account each time I look into the datasets, hence the android_apps_data[1:].


```python
android_clean = []
already_added = []

for app in android_apps_data[1:]:
    name = app[0]
    n_reviews = float(app[3])
    
    if (reviews_max[name] == n_reviews) and (name not in already_added):
        android_clean.append(app)
        already_added.append(name)
        
explore_data(android_clean, 0, 3, True)
```

    ['Photo Editor & Candy Camera & Grid & ScrapBook', 'ART_AND_DESIGN', '4.1', '159', '19M', '10,000+', 'Free', '0', 'Everyone', 'Art & Design', 'January 7, 2018', '1.0.0', '4.0.3 and up']
    
    
    ['U Launcher Lite – FREE Live Cool Themes, Hide Apps', 'ART_AND_DESIGN', '4.7', '87510', '8.7M', '5,000,000+', 'Free', '0', 'Everyone', 'Art & Design', 'August 1, 2018', '1.2.4', '4.0.3 and up']
    
    
    ['Sketch - Draw & Paint', 'ART_AND_DESIGN', '4.5', '215644', '25M', '50,000,000+', 'Free', '0', 'Teen', 'Art & Design', 'June 8, 2018', 'Varies with device', '4.2 and up']
    
    
    Number of rows: 9659
    Number of columns: 13


Now to check and make sure all the apps are in English and not a foreign language, since those are the types of apps being advertised in this pseudo-company. For both android and iOS apps, there are non-English speaking games.  In order to ensure they are all in English, a function must be created to filter out all non-English characters, based off the ASCII.


```python
def eng_string(string):
    for char in string:
        if ord(char) >= 127:
          return  False
    return True
    
print(eng_string("Instagram"))
print(eng_string("爱奇艺PPS -《欢乐颂2》电视剧热播"))
print(eng_string("Docs To Go™ Free Office Suite"))
print(eng_string("Instachat 😜"))
```

    True
    False
    False
    False


Since there are some characters, such as emojis and trademarks, that appear in app titles they must also be filtered through. Since they surpass the range in ASCII they are threatened to be eliminated incorrectly.


```python
def eng_string(string):
    non_ascii = 0
    for char in string:
        if ord(char) > 127:
            non_ascii += 1
    if non_ascii > 3:
        return False
    else:  
        return True
    
print(eng_string("爱奇艺PPS -《欢乐颂2》电视剧热播"))
print(eng_string("Docs To Go™ Free Office Suite"))
print(eng_string("Instachat 😜"))
```

    False
    True
    True


Now the function is a bit more effective at filtering out non-English speaking apps. This function is implemented for both iOS and android data sets.


```python
English_android_apps = []
english_apple_apps = []
for app in android_clean:
    name = app[0]
    test = eng_string(name)
    if test is True:
        English_android_apps.append(app)
        
for app in apple_apps_data[1:]:
    name = app[1]
    test = eng_string(name)
    if test is True:
        english_apple_apps.append(app)
        
explore_data(English_android_apps, 0, 3, True)
print('\n')
explore_data(english_apple_apps, 0, 3, True)
        
```

    ['Photo Editor & Candy Camera & Grid & ScrapBook', 'ART_AND_DESIGN', '4.1', '159', '19M', '10,000+', 'Free', '0', 'Everyone', 'Art & Design', 'January 7, 2018', '1.0.0', '4.0.3 and up']
    
    
    ['U Launcher Lite – FREE Live Cool Themes, Hide Apps', 'ART_AND_DESIGN', '4.7', '87510', '8.7M', '5,000,000+', 'Free', '0', 'Everyone', 'Art & Design', 'August 1, 2018', '1.2.4', '4.0.3 and up']
    
    
    ['Sketch - Draw & Paint', 'ART_AND_DESIGN', '4.5', '215644', '25M', '50,000,000+', 'Free', '0', 'Teen', 'Art & Design', 'June 8, 2018', 'Varies with device', '4.2 and up']
    
    
    Number of rows: 9614
    Number of columns: 13
    
    
    ['284882215', 'Facebook', '389879808', 'USD', '0.0', '2974676', '212', '3.5', '3.5', '95.0', '4+', 'Social Networking', '37', '1', '29', '1']
    
    
    ['389801252', 'Instagram', '113954816', 'USD', '0.0', '2161558', '1289', '4.5', '4.0', '10.23', '12+', 'Photo & Video', '37', '0', '29', '1']
    
    
    ['529479190', 'Clash of Clans', '116476928', 'USD', '0.0', '2130805', '579', '4.5', '4.5', '9.24.12', '9+', 'Games', '38', '5', '18', '1']
    
    
    Number of rows: 6183
    Number of columns: 16


The android_clean now only has the rows excluding the prevoius header that was on android_apps_data, therefore I no longer need to include "[1:]" after it. This also applies to any variable name that is a version of android_clean as well. apple_apps_data, however, still includes its header row and thus the "[1:]" is still necessary.

Now for the final step in data-cleaning: the non-free apps must be filtered out, so that only the free, english-speaking, non-duplicated apps remain. 


```python
free_android_apps = []
free_ios_apps = []
for app in English_android_apps:
    price = app[7]
    if price == '0':
        free_android_apps.append(app)
        
for app in english_apple_apps:
    price = app[4]
    if price == '0.0':
        free_ios_apps.append(app)
        
print(len(free_android_apps))
print(len(free_ios_apps))

        
```

    8864
    3222


# DATA ANALYSIS
## Finding the most profitable apps on iOS and Android

The aim of this pseudo-company is to determind the kinds of apps taht are likely to attract more users based on revenue and influence. The strategy to building a profitable app idea is the following steps:

1. Build a minimal version of the app on Android and add it to Google Play
2. If the app has good response from users, the app is developed further
3. If the app is profitable after six months, an iOS version of the app is built and added to the App Store

The goal is to find apps that are successful on both platforms.



```python
explore_data(free_android_apps, 0, 4)
print('\n')
explore_data(free_ios_apps, 0, 4)

```

    ['Photo Editor & Candy Camera & Grid & ScrapBook', 'ART_AND_DESIGN', '4.1', '159', '19M', '10,000+', 'Free', '0', 'Everyone', 'Art & Design', 'January 7, 2018', '1.0.0', '4.0.3 and up']
    
    
    ['U Launcher Lite – FREE Live Cool Themes, Hide Apps', 'ART_AND_DESIGN', '4.7', '87510', '8.7M', '5,000,000+', 'Free', '0', 'Everyone', 'Art & Design', 'August 1, 2018', '1.2.4', '4.0.3 and up']
    
    
    ['Sketch - Draw & Paint', 'ART_AND_DESIGN', '4.5', '215644', '25M', '50,000,000+', 'Free', '0', 'Teen', 'Art & Design', 'June 8, 2018', 'Varies with device', '4.2 and up']
    
    
    ['Pixel Draw - Number Art Coloring Book', 'ART_AND_DESIGN', '4.3', '967', '2.8M', '100,000+', 'Free', '0', 'Everyone', 'Art & Design;Creativity', 'June 20, 2018', '1.1', '4.4 and up']
    
    
    
    
    ['284882215', 'Facebook', '389879808', 'USD', '0.0', '2974676', '212', '3.5', '3.5', '95.0', '4+', 'Social Networking', '37', '1', '29', '1']
    
    
    ['389801252', 'Instagram', '113954816', 'USD', '0.0', '2161558', '1289', '4.5', '4.0', '10.23', '12+', 'Photo & Video', '37', '0', '29', '1']
    
    
    ['529479190', 'Clash of Clans', '116476928', 'USD', '0.0', '2130805', '579', '4.5', '4.5', '9.24.12', '9+', 'Games', '38', '5', '18', '1']
    
    
    ['420009108', 'Temple Run', '65921024', 'USD', '0.0', '1724546', '3842', '4.5', '4.0', '1.6.2', '9+', 'Games', '40', '5', '1', '1']
    
    


Column 9 is the genre for the android apps, and column 11 is the genre for iOS apps. The genre could be useful in analyzing which apps are more profitable.


```python
def freq_table(dataset, index):
    table = {}
    total = 0
    for app in dataset:
        total += 1
        item = app[index]
        if item not in table:
            table[item] = 1
        else:
            table[item] += 1
            
    table_percentages = {}
    for key in table:
        percentages = (table[key]/total) *100
        table_percentages[key] = percentages
    return table_percentages
            
def display_table(dataset, index):
    table = freq_table(dataset, index)
    table_display = []
    for key in table:
        key_as_tuple = (table[key], key)
        table_display.append(key_as_tuple)
        
    sorted_table = sorted(table_display, reverse = True)
    for entry in sorted_table:
        print(entry[1], ":", entry[0])
```


```python
display_table(free_ios_apps[1:], 11)
```

    Games : 58.180689226948154
    Entertainment : 7.885749767153058
    Photo & Video : 4.967401428127911
    Education : 3.6634585532443342
    Social Networking : 3.2598571872089415
    Shopping : 2.607885749767153
    Utilities : 2.5147469729897547
    Sports : 2.1421918658801617
    Music : 2.049053089102763
    Health & Fitness : 2.018006830176964
    Productivity : 1.7385904998447685
    Lifestyle : 1.5833592052157717
    News : 1.334989133809376
    Travel : 1.2418503570319777
    Finance : 1.11766532132878
    Weather : 0.8692952499223843
    Food & Drink : 0.8072027320707855
    Reference : 0.55883266066439
    Business : 0.5277864017385905
    Book : 0.43464762496119214
    Navigation : 0.18627755355479667
    Medical : 0.18627755355479667
    Catalogs : 0.12418503570319776


### For iOS apps:
The most common genre of apps is Games, with Entertainment being the second most but with a significantly lower percentage. Social Networking is trumped by Education and Photo & Video apps. It would seem that most apps are designed for Entertainment purposes rather than practical purposes. Since Games is far ahead of any other type of genre by over 50%, it would seem a larger number of users have Game apps. However, it could just be in comparison to the other types of apps that this is true and it may not have a significantly larger number of users. This is useful information in trying to see what type of apps are the most profitable.


```python
display_table(free_android_apps, 1)
```

    FAMILY : 18.907942238267147
    GAME : 9.724729241877256
    TOOLS : 8.461191335740072
    BUSINESS : 4.591606498194946
    LIFESTYLE : 3.9034296028880866
    PRODUCTIVITY : 3.892148014440433
    FINANCE : 3.7003610108303246
    MEDICAL : 3.531137184115524
    SPORTS : 3.395758122743682
    PERSONALIZATION : 3.3167870036101084
    COMMUNICATION : 3.2378158844765346
    HEALTH_AND_FITNESS : 3.0798736462093865
    PHOTOGRAPHY : 2.944494584837545
    NEWS_AND_MAGAZINES : 2.7978339350180503
    SOCIAL : 2.6624548736462095
    TRAVEL_AND_LOCAL : 2.33528880866426
    SHOPPING : 2.2450361010830324
    BOOKS_AND_REFERENCE : 2.1435018050541514
    DATING : 1.861462093862816
    VIDEO_PLAYERS : 1.7937725631768955
    MAPS_AND_NAVIGATION : 1.3989169675090252
    FOOD_AND_DRINK : 1.2409747292418771
    EDUCATION : 1.1620036101083033
    ENTERTAINMENT : 0.9589350180505415
    LIBRARIES_AND_DEMO : 0.9363718411552346
    AUTO_AND_VEHICLES : 0.9250902527075812
    HOUSE_AND_HOME : 0.8235559566787004
    WEATHER : 0.8009927797833934
    EVENTS : 0.7107400722021661
    PARENTING : 0.6543321299638989
    ART_AND_DESIGN : 0.6430505415162455
    COMICS : 0.6204873646209386
    BEAUTY : 0.5979241877256317


### For Android apps:

The categories are arranged differently than for iOS genres. The percentages of the frequency of apps are more distributed, instead of the lopsidedness of  the iOS apps towards Games. The FAMILY category has the most at 18%, followed by GAMING and TOOLS. Although gaming is still one of the top apps, it is not the top app for Android which seems to have apps for more practical uses than entertainment. The frequency table for android may have generated what genres have the most users, whereas the frequency table for iOS apps may have generated the most frequent app genres.


```python
display_table(free_android_apps, 9)
```

    Tools : 8.449909747292418
    Entertainment : 6.069494584837545
    Education : 5.347472924187725
    Business : 4.591606498194946
    Productivity : 3.892148014440433
    Lifestyle : 3.892148014440433
    Finance : 3.7003610108303246
    Medical : 3.531137184115524
    Sports : 3.463447653429603
    Personalization : 3.3167870036101084
    Communication : 3.2378158844765346
    Action : 3.1024368231046933
    Health & Fitness : 3.0798736462093865
    Photography : 2.944494584837545
    News & Magazines : 2.7978339350180503
    Social : 2.6624548736462095
    Travel & Local : 2.3240072202166067
    Shopping : 2.2450361010830324
    Books & Reference : 2.1435018050541514
    Simulation : 2.0419675090252705
    Dating : 1.861462093862816
    Arcade : 1.8501805054151623
    Video Players & Editors : 1.7712093862815883
    Casual : 1.7599277978339352
    Maps & Navigation : 1.3989169675090252
    Food & Drink : 1.2409747292418771
    Puzzle : 1.128158844765343
    Racing : 0.9927797833935018
    Role Playing : 0.9363718411552346
    Libraries & Demo : 0.9363718411552346
    Auto & Vehicles : 0.9250902527075812
    Strategy : 0.9138086642599278
    House & Home : 0.8235559566787004
    Weather : 0.8009927797833934
    Events : 0.7107400722021661
    Adventure : 0.6768953068592057
    Comics : 0.6092057761732852
    Beauty : 0.5979241877256317
    Art & Design : 0.5979241877256317
    Parenting : 0.4963898916967509
    Card : 0.45126353790613716
    Casino : 0.42870036101083037
    Trivia : 0.41741877256317694
    Educational;Education : 0.39485559566787
    Board : 0.3835740072202166
    Educational : 0.3722924187725632
    Education;Education : 0.33844765342960287
    Word : 0.2594765342960289
    Casual;Pretend Play : 0.236913357400722
    Music : 0.2030685920577617
    Racing;Action & Adventure : 0.16922382671480143
    Puzzle;Brain Games : 0.16922382671480143
    Entertainment;Music & Video : 0.16922382671480143
    Casual;Brain Games : 0.13537906137184114
    Casual;Action & Adventure : 0.13537906137184114
    Arcade;Action & Adventure : 0.12409747292418773
    Action;Action & Adventure : 0.10153429602888085
    Educational;Pretend Play : 0.09025270758122744
    Simulation;Action & Adventure : 0.078971119133574
    Parenting;Education : 0.078971119133574
    Entertainment;Brain Games : 0.078971119133574
    Board;Brain Games : 0.078971119133574
    Parenting;Music & Video : 0.06768953068592057
    Educational;Brain Games : 0.06768953068592057
    Casual;Creativity : 0.06768953068592057
    Art & Design;Creativity : 0.06768953068592057
    Education;Pretend Play : 0.056407942238267145
    Role Playing;Pretend Play : 0.04512635379061372
    Education;Creativity : 0.04512635379061372
    Role Playing;Action & Adventure : 0.033844765342960284
    Puzzle;Action & Adventure : 0.033844765342960284
    Entertainment;Creativity : 0.033844765342960284
    Entertainment;Action & Adventure : 0.033844765342960284
    Educational;Creativity : 0.033844765342960284
    Educational;Action & Adventure : 0.033844765342960284
    Education;Music & Video : 0.033844765342960284
    Education;Brain Games : 0.033844765342960284
    Education;Action & Adventure : 0.033844765342960284
    Adventure;Action & Adventure : 0.033844765342960284
    Video Players & Editors;Music & Video : 0.02256317689530686
    Sports;Action & Adventure : 0.02256317689530686
    Simulation;Pretend Play : 0.02256317689530686
    Puzzle;Creativity : 0.02256317689530686
    Music;Music & Video : 0.02256317689530686
    Entertainment;Pretend Play : 0.02256317689530686
    Casual;Education : 0.02256317689530686
    Board;Action & Adventure : 0.02256317689530686
    Video Players & Editors;Creativity : 0.01128158844765343
    Trivia;Education : 0.01128158844765343
    Travel & Local;Action & Adventure : 0.01128158844765343
    Tools;Education : 0.01128158844765343
    Strategy;Education : 0.01128158844765343
    Strategy;Creativity : 0.01128158844765343
    Strategy;Action & Adventure : 0.01128158844765343
    Simulation;Education : 0.01128158844765343
    Role Playing;Brain Games : 0.01128158844765343
    Racing;Pretend Play : 0.01128158844765343
    Puzzle;Education : 0.01128158844765343
    Parenting;Brain Games : 0.01128158844765343
    Music & Audio;Music & Video : 0.01128158844765343
    Lifestyle;Pretend Play : 0.01128158844765343
    Lifestyle;Education : 0.01128158844765343
    Health & Fitness;Education : 0.01128158844765343
    Health & Fitness;Action & Adventure : 0.01128158844765343
    Entertainment;Education : 0.01128158844765343
    Communication;Creativity : 0.01128158844765343
    Comics;Creativity : 0.01128158844765343
    Casual;Music & Video : 0.01128158844765343
    Card;Action & Adventure : 0.01128158844765343
    Books & Reference;Education : 0.01128158844765343
    Art & Design;Pretend Play : 0.01128158844765343
    Art & Design;Action & Adventure : 0.01128158844765343
    Arcade;Pretend Play : 0.01128158844765343
    Adventure;Education : 0.01128158844765343


The 'Genres' column for Android reiterates the fact that the most common apps are more practical than entertainment, with Tools being the highest percentage at 8%, followed by Entertainment. Even though entertainment is still high, the majority of the type of apps in the high percentages are more practical (i.e. Business, Productivity, Education, etc). The difference between 'Category' and 'Genres' columns is significant but unclear, there is just a lot more Genres than Categories. Again, this table may illustrate more of what genres have the most users.

#### Average Installs

Now to calculate the average number of installs for each genre, which should correlate to which apps are the most popular.


```python
ios_table = freq_table(free_ios_apps, -5)

for genre in ios_table:
    total = 0
    len_genre = 0
    
    for app in free_ios_apps:
        genre_app = app[-5]
        if genre_app == genre:
            num_ratings = float(app[5])
            total += num_ratings
            len_genre += 1
            
    avg_users = total / len_genre
    print(genre,":", avg_users)

```

    Utilities : 18684.456790123455
    Medical : 612.0
    Sports : 23008.898550724636
    News : 21248.023255813954
    Travel : 28243.8
    Business : 7491.117647058823
    Productivity : 21028.410714285714
    Health & Fitness : 23298.015384615384
    Music : 57326.530303030304
    Games : 22788.6696905016
    Finance : 31467.944444444445
    Lifestyle : 16485.764705882353
    Photo & Video : 28441.54375
    Navigation : 86090.33333333333
    Social Networking : 71548.34905660378
    Food & Drink : 33333.92307692308
    Shopping : 26919.690476190477
    Education : 7003.983050847458
    Weather : 52279.892857142855
    Reference : 74942.11111111111
    Entertainment : 14029.830708661417
    Book : 39758.5
    Catalogs : 4004.0


It appears that Navigation has the highest number of ratings, meaning the highest users. Other entertainment type of apps, such as Social Networking, Photo & Video, Games and Music, also have a high amount of users. However, more practical apps dominate the number of users such as References and Weather which are ranked second and third highest users, respectively. Food and Drink and Finance are also amongst the most popular apps. 

Each type of app is fair game, but a recommended app profile would be one that is mainly a reference app with an incorporated social media aspect. That way this more practical app would stand out and people are more likely to share and discuss online, spreading the news of the app and therefore generating more users.


```python
for app in free_ios_apps:
    if app[-5] == 'Navigation':
        print(app[1], ":", app[5])
```

    Waze - GPS Navigation, Maps & Real-time Traffic : 345046
    Google Maps - Navigation & Transit : 154911
    Geocaching® : 12811
    CoPilot GPS – Car Navigation & Offline Maps : 3582
    ImmobilienScout24: Real Estate Search in Germany : 187
    Railway Route Search : 5


Looking deeper into the top genre of apps, it seems that a majority of the ratings are for only two apps: Waze and Google Maps, which make up more than 50% of the ratings for this genre. This causes the results to be slightly skewed based on these results. The same can be said for Social Media apps and References.


```python
for app in free_ios_apps:
    if app[-5] == 'Reference':
        print(app[1], ":", app[5])
```

    Bible : 985920
    Dictionary.com Dictionary & Thesaurus : 200047
    Dictionary.com Dictionary & Thesaurus for iPad : 54175
    Google Translate : 26786
    Muslim Pro: Ramadan 2017 Prayer Times, Azan, Quran : 18418
    New Furniture Mods - Pocket Wiki & Game Tools for Minecraft PC Edition : 17588
    Merriam-Webster Dictionary : 16849
    Night Sky : 12122
    City Maps for Minecraft PE - The Best Maps for Minecraft Pocket Edition (MCPE) : 8535
    LUCKY BLOCK MOD ™ for Minecraft PC Edition - The Best Pocket Wiki & Mods Installer Tools : 4693
    GUNS MODS for Minecraft PC Edition - Mods Tools : 1497
    Guides for Pokémon GO - Pokemon GO News and Cheats : 826
    WWDC : 762
    Horror Maps for Minecraft PE - Download The Scariest Maps for Minecraft Pocket Edition (MCPE) Free : 718
    VPN Express : 14
    Real Bike Traffic Rider Virtual Reality Glasses : 8
    教えて!goo : 0
    Jishokun-Japanese English Dictionary & Translator : 0



```python
for app in free_ios_apps:
    if app[-5] == 'Social Networking':
        print(app[1], ":", app[5])
```

    Facebook : 2974676
    Pinterest : 1061624
    Skype for iPhone : 373519
    Messenger : 351466
    Tumblr : 334293
    WhatsApp Messenger : 287589
    Kik : 260965
    ooVoo – Free Video Call, Text and Voice : 177501
    TextNow - Unlimited Text + Calls : 164963
    Viber Messenger – Text & Call : 164249
    Followers - Social Analytics For Instagram : 112778
    MeetMe - Chat and Meet New People : 97072
    We Heart It - Fashion, wallpapers, quotes, tattoos : 90414
    InsTrack for Instagram - Analytics Plus More : 85535
    Tango - Free Video Call, Voice and Chat : 75412
    LinkedIn : 71856
    Match™ - #1 Dating App. : 60659
    Skype for iPad : 60163
    POF - Best Dating App for Conversations : 52642
    Timehop : 49510
    Find My Family, Friends & iPhone - Life360 Locator : 43877
    Whisper - Share, Express, Meet : 39819
    Hangouts : 36404
    LINE PLAY - Your Avatar World : 34677
    WeChat : 34584
    Badoo - Meet New People, Chat, Socialize. : 34428
    Followers + for Instagram - Follower Analytics : 28633
    GroupMe : 28260
    Marco Polo Video Walkie Talkie : 27662
    Miitomo : 23965
    SimSimi : 23530
    Grindr - Gay and same sex guys chat, meet and date : 23201
    Wishbone - Compare Anything : 20649
    imo video calls and chat : 18841
    After School - Funny Anonymous School News : 18482
    Quick Reposter - Repost, Regram and Reshare Photos : 17694
    Weibo HD : 16772
    Repost for Instagram : 15185
    Live.me – Live Video Chat & Make Friends Nearby : 14724
    Nextdoor : 14402
    Followers Analytics for Instagram - InstaReport : 13914
    YouNow: Live Stream Video Chat : 12079
    FollowMeter for Instagram - Followers Tracking : 11976
    LINE : 11437
    eHarmony™ Dating App - Meet Singles : 11124
    Discord - Chat for Gamers : 9152
    QQ : 9109
    Telegram Messenger : 7573
    Weibo : 7265
    Periscope - Live Video Streaming Around the World : 6062
    Chat for Whatsapp - iPad Version : 5060
    QQ HD : 5058
    Followers Analysis Tool For Instagram App Free : 4253
    live.ly - live video streaming : 4145
    Houseparty - Group Video Chat : 3991
    SOMA Messenger : 3232
    Monkey : 3060
    Down To Lunch : 2535
    Flinch - Video Chat Staring Contest : 2134
    Highrise - Your Avatar Community : 2011
    LOVOO - Dating Chat : 1985
    PlayStation®Messages : 1918
    BOO! - Video chat camera with filters & stickers : 1805
    Qzone : 1649
    Chatous - Chat with new people : 1609
    Kiwi - Q&A : 1538
    GhostCodes - a discovery app for Snapchat : 1313
    Jodel : 1193
    FireChat : 1037
    Google Duo - simple video calling : 1033
    Fiesta by Tango - Chat & Meet New People : 885
    Google Allo — smart messaging : 862
    Peach — share vividly : 727
    Hey! VINA - Where Women Meet New Friends : 719
    Battlefield™ Companion : 689
    All Devices for WhatsApp - Messenger for iPad : 682
    Chat for Pokemon Go - GoChat : 500
    IAmNaughty – Dating App to Meet New People Online : 463
    Qzone HD : 458
    Zenly - Locate your friends in realtime : 427
    League of Legends Friends : 420
    豆瓣 : 407
    Candid - Speak Your Mind Freely : 398
    知乎 : 397
    Selfeo : 366
    Fake-A-Location Free ™ : 354
    Popcorn Buzz - Free Group Calls : 281
    Fam — Group video calling for iMessage : 279
    QQ International : 274
    Ameba : 269
    SoundCloud Pulse: for creators : 240
    Tantan : 235
    Cougar Dating & Life Style App for Mature Women : 213
    Rawr Messenger - Dab your chat : 180
    WhenToPost: Best Time to Post Photos for Instagram : 158
    Inke—Broadcast an amazing life : 147
    Mustknow - anonymous video Q&A : 53
    CTFxCmoji : 39
    Lobi : 36
    Chain: Collaborate On MyVideo Story/Group Video : 35
    botman - Real time video chat : 7
    BestieBox : 0
    MATCH ON LINE chat : 0
    niconico ch : 0
    LINE BLOG : 0
    bit-tube - Live Stream Video Chat : 0


Even though the results have a bit of skew, I would still recommend a social networking reference app.

Now to analyze the Google Play apps similarly.


```python
android_table = freq_table(free_android_apps, 1)

for category in android_table:
    total = 0
    len_category = 0
        
    for app in free_android_apps:
        category_app = app[1]
        if category_app == category:
            num_installs = app[5]
            num_installs = num_installs.replace("+", '')
            num_installs = num_installs.replace(",", '')
            num_installs = float(num_installs)
            total += num_installs
            len_category += 1
            
    avg_installs = total / len_category
    print(category, ":", avg_installs)

```

    LIFESTYLE : 1437816.2687861272
    PRODUCTIVITY : 16787331.344927534
    SOCIAL : 23253652.127118643
    WEATHER : 5074486.197183099
    PHOTOGRAPHY : 17840110.40229885
    DATING : 854028.8303030303
    GAME : 15588015.603248259
    EVENTS : 253542.22222222222
    TRAVEL_AND_LOCAL : 13984077.710144928
    SHOPPING : 7036877.311557789
    MAPS_AND_NAVIGATION : 4056941.7741935486
    LIBRARIES_AND_DEMO : 638503.734939759
    FAMILY : 3695641.8198090694
    ENTERTAINMENT : 11640705.88235294
    BUSINESS : 1712290.1474201474
    FINANCE : 1387692.475609756
    VIDEO_PLAYERS : 24727872.452830188
    EDUCATION : 1833495.145631068
    BEAUTY : 513151.88679245283
    ART_AND_DESIGN : 1986335.0877192982
    NEWS_AND_MAGAZINES : 9549178.467741935
    AUTO_AND_VEHICLES : 647317.8170731707
    COMICS : 817657.2727272727
    COMMUNICATION : 38456119.167247385
    HOUSE_AND_HOME : 1331540.5616438356
    SPORTS : 3638640.1428571427
    FOOD_AND_DRINK : 1924897.7363636363
    PARENTING : 542603.6206896552
    BOOKS_AND_REFERENCE : 8767811.894736841
    TOOLS : 10801391.298666667
    PERSONALIZATION : 5201482.6122448975
    MEDICAL : 120550.61980830671
    HEALTH_AND_FITNESS : 4188821.9853479853


For Android apps, the COMMUNICATION genre seems to have the most installs and therefore the most users, with over 38 million installations. Other top installed apps include SOCIAL and VIDEO_PLAYERS. Let's look more into these categories to see if there is also a skew of data. 


```python
for app in free_android_apps:
    if app[1] == 'COMMUNICATION' and (app[5] == '1,000,000,000+' or app[5] == '500,000,000+' or app[5] == '100,000,000+'):
        print(app[0], ":", app[5])
```

    WhatsApp Messenger : 1,000,000,000+
    imo beta free calls and text : 100,000,000+
    Android Messages : 100,000,000+
    Google Duo - High Quality Video Calls : 500,000,000+
    Messenger – Text and Video Chat for Free : 1,000,000,000+
    imo free video calls and chat : 500,000,000+
    Skype - free IM & video calls : 1,000,000,000+
    Who : 100,000,000+
    GO SMS Pro - Messenger, Free Themes, Emoji : 100,000,000+
    LINE: Free Calls & Messages : 500,000,000+
    Google Chrome: Fast & Secure : 1,000,000,000+
    Firefox Browser fast & private : 100,000,000+
    UC Browser - Fast Download Private & Secure : 500,000,000+
    Gmail : 1,000,000,000+
    Hangouts : 1,000,000,000+
    Messenger Lite: Free Calls & Messages : 100,000,000+
    Kik : 100,000,000+
    KakaoTalk: Free Calls & Text : 100,000,000+
    Opera Mini - fast web browser : 100,000,000+
    Opera Browser: Fast and Secure : 100,000,000+
    Telegram : 100,000,000+
    Truecaller: Caller ID, SMS spam blocking & Dialer : 100,000,000+
    UC Browser Mini -Tiny Fast Private & Secure : 100,000,000+
    Viber Messenger : 500,000,000+
    WeChat : 100,000,000+
    Yahoo Mail – Stay Organized : 100,000,000+
    BBM - Free Calls & Messages : 100,000,000+



```python

```


```python
for app in free_android_apps:
    if app[1] == 'VIDEO_PLAYERS' and (app[5] == '1,000,000,000+' or app[5] == '500,000,000+' or app[5] == '100,000,000+'):
        print(app[0], ":", app[5])
```

    YouTube : 1,000,000,000+
    Motorola Gallery : 100,000,000+
    VLC for Android : 100,000,000+
    Google Play Movies & TV : 1,000,000,000+
    MX Player : 500,000,000+
    Dubsmash : 100,000,000+
    VivaVideo - Video Editor & Photo Movie : 100,000,000+
    VideoShow-Video Editor, Video Maker, Beauty Camera : 100,000,000+
    Motorola FM Radio : 100,000,000+



```python
for app in free_android_apps:
    if app[1] == 'SOCIAL' and (app[5] == '1,000,000,000+' or app[5] == '500,000,000+' or app[5] == '100,000,000+'):
        print(app[0], ":", app[5])
```

    Facebook : 1,000,000,000+
    Facebook Lite : 500,000,000+
    Tumblr : 100,000,000+
    Pinterest : 100,000,000+
    Google+ : 1,000,000,000+
    Badoo - Free Chat & Dating App : 100,000,000+
    Tango - Live Video Broadcast : 100,000,000+
    Instagram : 1,000,000,000+
    Snapchat : 500,000,000+
    LinkedIn : 100,000,000+
    Tik Tok - including musical.ly : 100,000,000+
    BIGO LIVE - Live Stream : 100,000,000+
    VK : 100,000,000+


I narrowed the results to only show installs of over 100 million, since those are the ones most likely to skew the data. The more apps with over 1 billion installs are the main outliers that will skew the data the most. For each of the top categories there are 2-3 apps that have over 1 billion downloads, such as WhatsApp, Messenger, Skype and Google Chrome, etc. for COMMUNICATION, YouTube and Google Play Movies & TV for VIDEO_PLAYERS, and Facebook, Instagram and Google+ for SOCIAL. 

Communication through video and social networking seems to be a good idea for an app for Google Play. One could still use a more practical type of app and incorporate these ideas to get the most out of the app's profitability. Using the references category one could take that and create an app based off what is the most popular kind of reference or book. The reference app should include more communication, maybe a social networking aspect to display moods on the reference book sections
