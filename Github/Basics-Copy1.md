
# Comparing Hacker News Posts

This project takes 20,000 data from a website called Hacker News where users create posts and comment on others, similar to reddit. In this project, I will compare two types of posts, Ask HN and Show HN, to see which posts receive the most comments on average and what time receives more comments on average. The differene between the two types of posts is that one ask a question (Ask HN), and the other conveys information (Show HN).


```python
from csv import reader
opened_file = open("hacker_news.csv")
read_file = reader(opened_file)
hn = list(read_file)

print(hn[:5]) #displays first five rows of hn

```

    [['id', 'title', 'url', 'num_points', 'num_comments', 'author', 'created_at'], ['12224879', 'Interactive Dynamic Video', 'http://www.interactivedynamicvideo.com/', '386', '52', 'ne0phyte', '8/4/2016 11:52'], ['10975351', 'How to Use Open Source and Shut the Fuck Up at the Same Time', 'http://hueniverse.com/2016/01/26/how-to-use-open-source-and-shut-the-fuck-up-at-the-same-time/', '39', '10', 'josep2', '1/26/2016 19:30'], ['11964716', "Florida DJs May Face Felony for April Fools' Water Joke", 'http://www.thewire.com/entertainment/2013/04/florida-djs-april-fools-water-joke/63798/', '2', '1', 'vezycash', '6/23/2016 22:20'], ['11919867', 'Technology ventures: From Idea to Enterprise', 'https://www.amazon.com/Technology-Ventures-Enterprise-Thomas-Byers/dp/0073523429', '3', '1', 'hswarna', '6/17/2016 0:01']]



```python
headers = hn[0] #seperate and remove header row
hn = hn[1:]

print(headers, '\n')
print(hn[:5])
```

    ['id', 'title', 'url', 'num_points', 'num_comments', 'author', 'created_at'] 
    
    [['12224879', 'Interactive Dynamic Video', 'http://www.interactivedynamicvideo.com/', '386', '52', 'ne0phyte', '8/4/2016 11:52'], ['10975351', 'How to Use Open Source and Shut the Fuck Up at the Same Time', 'http://hueniverse.com/2016/01/26/how-to-use-open-source-and-shut-the-fuck-up-at-the-same-time/', '39', '10', 'josep2', '1/26/2016 19:30'], ['11964716', "Florida DJs May Face Felony for April Fools' Water Joke", 'http://www.thewire.com/entertainment/2013/04/florida-djs-april-fools-water-joke/63798/', '2', '1', 'vezycash', '6/23/2016 22:20'], ['11919867', 'Technology ventures: From Idea to Enterprise', 'https://www.amazon.com/Technology-Ventures-Enterprise-Thomas-Byers/dp/0073523429', '3', '1', 'hswarna', '6/17/2016 0:01'], ['10301696', 'Note by Note: The Making of Steinway L1037 (2007)', 'http://www.nytimes.com/2007/11/07/movies/07stein.html?_r=0', '8', '2', 'walterbell', '9/30/2015 4:12']]



```python
## Now to filter out the two types of data from the posts: Ask HN and Show HN
ask_posts = []
show_posts = []
other_posts = []

for row in hn:
    title = row[1]
    title = title.lower()
    
    if title.startswith('ask hn') == True:
        ask_posts.append(row)
    elif title.startswith('show hn') == True:
        show_posts.append(row)
    else:
        other_posts.append(row)
        
print('Ask:', len(ask_posts))
print('Show:', len(show_posts))
print('Other:', len(other_posts))
```

    Ask: 1744
    Show: 1162
    Other: 17194



```python
print(ask_posts[:5], '\n')
print(show_posts[:5])
```

    [['12296411', 'Ask HN: How to improve my personal website?', '', '2', '6', 'ahmedbaracat', '8/16/2016 9:55'], ['10610020', 'Ask HN: Am I the only one outraged by Twitter shutting down share counts?', '', '28', '29', 'tkfx', '11/22/2015 13:43'], ['11610310', 'Ask HN: Aby recent changes to CSS that broke mobile?', '', '1', '1', 'polskibus', '5/2/2016 10:14'], ['12210105', 'Ask HN: Looking for Employee #3 How do I do it?', '', '1', '3', 'sph130', '8/2/2016 14:20'], ['10394168', 'Ask HN: Someone offered to buy my browser extension from me. What now?', '', '28', '17', 'roykolak', '10/15/2015 16:38']] 
    
    [['10627194', 'Show HN: Wio Link  ESP8266 Based Web of Things Hardware Development Platform', 'https://iot.seeed.cc', '26', '22', 'kfihihc', '11/25/2015 14:03'], ['10646440', 'Show HN: Something pointless I made', 'http://dn.ht/picklecat/', '747', '102', 'dhotson', '11/29/2015 22:46'], ['11590768', 'Show HN: Shanhu.io, a programming playground powered by e8vm', 'https://shanhu.io', '1', '1', 'h8liu', '4/28/2016 18:05'], ['12178806', 'Show HN: Webscope  Easy way for web developers to communicate with Clients', 'http://webscopeapp.com', '3', '3', 'fastbrick', '7/28/2016 7:11'], ['10872799', 'Show HN: GeoScreenshot  Easily test Geo-IP based web pages', 'https://www.geoscreenshot.com/', '1', '9', 'kpsychwave', '1/9/2016 20:45']]



```python
# Find the total number of comments for each post type we are interested in
#ask comments
total_ask_comments = 0
for row in ask_posts:
    num_comments = row[4]
    num_comments = int(num_comments)
    total_ask_comments += num_comments
    
print(total_ask_comments)
avg_ask_comments = total_ask_comments / len(ask_posts)
print('Average Ask comments:', avg_ask_comments)

#show comments
total_show_comments = 0
for row in show_posts:
    num_comments = row[4]
    num_comments = int(num_comments)
    total_show_comments += num_comments

print(total_show_comments)
avg_show_comments = total_show_comments / len(show_posts)
print('Average Show comments:', avg_show_comments)
```

    24483
    Average Ask comments: 14.038417431192661
    11988
    Average Show comments: 10.31669535283993


It appears that Ask posts receive more comments than Show posts, with almost more than twice the amount of comments for Ask posts than Show posts. This makes sense because people are more likely to comment an answer to a question in order to help someone out, whereas show posts which are just demonstrating a project, product or interesting idea may simply be glanced at with no commentary.


```python
#Narrowing down the times for most comments on Ask posts
import datetime as dt
result_list = []

for row in ask_posts:
    created_at = row[6]
    num_comments = row[4]
    num_comments = int(num_comments)
    result_list.append([created_at, num_comments])

counts_by_hour = {}
comments_by_hour = {}

for row in result_list:
    hours = row[0]
    comments = row[1]
    hours = str(hours)
    date = dt.datetime.strptime(hours, "%m/%d/%Y %H:%M")
    hour_str = date.strftime("%H")
    
    if hour_str not in counts_by_hour:
        counts_by_hour[hour_str] = 1
        comments_by_hour[hour_str] = comments
    else:
        counts_by_hour[hour_str] += 1
        comments_by_hour[hour_str] += comments
        
print(counts_by_hour)
print(comments_by_hour)
```

    {'16': 108, '09': 45, '22': 71, '15': 116, '19': 110, '02': 58, '13': 85, '07': 34, '12': 73, '00': 55, '08': 48, '11': 58, '18': 109, '03': 54, '14': 107, '20': 80, '06': 44, '23': 68, '17': 100, '05': 46, '21': 109, '10': 59, '04': 47, '01': 60}
    {'16': 1814, '09': 251, '22': 479, '15': 4477, '19': 1188, '02': 1381, '13': 1253, '07': 267, '12': 687, '00': 447, '08': 492, '11': 641, '18': 1439, '03': 421, '14': 1416, '20': 1722, '06': 397, '23': 543, '17': 1146, '05': 464, '21': 1745, '10': 793, '04': 337, '01': 683}



```python
avg_by_hour = []


for hr in counts_by_hour:
    avg_by_hour.append([hr, comments_by_hour[hr]/counts_by_hour[hr]])
    
print(avg_by_hour)
    
```

    [['16', 16.796296296296298], ['09', 5.5777777777777775], ['22', 6.746478873239437], ['15', 38.5948275862069], ['19', 10.8], ['02', 23.810344827586206], ['13', 14.741176470588234], ['07', 7.852941176470588], ['12', 9.41095890410959], ['00', 8.127272727272727], ['08', 10.25], ['11', 11.051724137931034], ['18', 13.20183486238532], ['03', 7.796296296296297], ['14', 13.233644859813085], ['20', 21.525], ['06', 9.022727272727273], ['23', 7.985294117647059], ['17', 11.46], ['05', 10.08695652173913], ['21', 16.009174311926607], ['10', 13.440677966101696], ['04', 7.170212765957447], ['01', 11.383333333333333]]



```python
#To make list more readable
swap_avg_by_hour = []
for row in avg_by_hour:
    e2 = row[0]
    e1 = row[1]
    swap_avg_by_hour.append([e1, e2])
    
print(swap_avg_by_hour)

sorted_swap = sorted(swap_avg_by_hour, reverse=True)

print('\n',"Top 5 Hours for Ask Posts Comments:")
print(sorted_swap[:5], '\n')

for avg, hr in sorted_swap[:5]:
    time = dt.datetime.strptime(hr, "%H")
    hour = time.strftime('%H:%M')
    print("{}: {:.2f} average comments per post".format (hour, avg))

    
```

    [[16.796296296296298, '16'], [5.5777777777777775, '09'], [6.746478873239437, '22'], [38.5948275862069, '15'], [10.8, '19'], [23.810344827586206, '02'], [14.741176470588234, '13'], [7.852941176470588, '07'], [9.41095890410959, '12'], [8.127272727272727, '00'], [10.25, '08'], [11.051724137931034, '11'], [13.20183486238532, '18'], [7.796296296296297, '03'], [13.233644859813085, '14'], [21.525, '20'], [9.022727272727273, '06'], [7.985294117647059, '23'], [11.46, '17'], [10.08695652173913, '05'], [16.009174311926607, '21'], [13.440677966101696, '10'], [7.170212765957447, '04'], [11.383333333333333, '01']]
    
     Top 5 Hours for Ask Posts Comments:
    [[38.5948275862069, '15'], [23.810344827586206, '02'], [21.525, '20'], [16.796296296296298, '16'], [16.009174311926607, '21']] 
    
    15:00: 38.59 average comments per post
    02:00: 23.81 average comments per post
    20:00: 21.52 average comments per post
    16:00: 16.80 average comments per post
    21:00: 16.01 average comments per post


## Analysis
According to the data, 3pm EST is the most popular time to post to get the most comments, followed by 2am, 8pm, and 4pm all in Eastern Standard Time. During this time, people may be getting ready to get off of work, or staying up late at night browsing and come across such posts. People apper to be more inclined to leave comments during these times. So if you want a response from someone on a question you have on Hacker News, then the best bet would be to post around 3-4pm EST.
