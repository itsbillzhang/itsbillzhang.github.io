---
published: true
---
<div class="featured">
<a href="{{ page.url }}">
<img src="{{site.url}}/images/Friends.jpg" />
</a>
</div>

Code found [here](https://github.com/itsbillzhang/MakeAFriend).

## Motivation

Although everyone needs friends, for a cascade of reasons, many struggle to make them. With COVID19 in full-swing, I realized that many are isolated at home (or away from home) with expropriated feelings of loneliness, feelings that would be intensified if they didn’t have anyone to talk to. 

Hoping to connect more strangers to be friends, I reminded myself that one could easily use Tinder, Bumble, or ...FarmersOnly instead, platforms that give its users abundant choice to do that. However, what’s common on all these platforms is that information on the other person is very superficial, and with an endless amount of other users to swipe through, each connection made feels meaningless. Therefore, friend pairing would not just pair strangers randomly but connect optimal matches. Excited, I tried my hand at building a program that given user data through signups, would find ideas an ideal friend in a pool of candidates.


<p class="centered-text">
<img class="centered" src="{{site.url}}/images/tinder.PNG" />
</p>

## Collecting User Responses

I started by thinking the audience that I wanted to reach. I concluded that the audience would be users of a University of Waterloo forum ([/r/uwaterloo](https://www.reddit.com/r/uwaterloo/)), as it offered me anonymity, the users would be of similar demographic and proximity, and the users were very supportive of me running such as a project.

I then had to figure out what responses I wanted to collect. On the forum, I posted a [Google Docs](https://docs.google.com/document/d/1S8X0gCyoaxQ8XP4K4SaEApPJVUt2rtwwkkIXDiyuDCQ/edit) where I started with questions that would help determine compatibility and encouraged users to add questions they wanted to see in the match generation. In the end, I chose 30 out of 73 questions, all answered from a 1 to 7 scale. Some of the questions chosen were:

- “I’m up for watching a movie over the internet with my partner.”
- “I want to communicate often with my partner”
- “I enjoy deeper conversations more than light-hearted ones.”


Paperform.co was picked as a way to collect user responses, as it was a free alternative to Typeform. On the form, I asked users for their @uwaterloo email for validation, their gender, sex, responses to the 30 questions, and their hobbies. Thanks to Paperform, all responses were stored on a .csv file. I also created a [information and data policy](https://docs.google.com/document/d/1Kn6kTAUMRZpxLhtcIFPy98o4381oau4aRQb-xxq9wEY/edit?usp=sharing) on what I intended to do with their confidential information, one which I intended to follow. After posting the Paperform link, the project was now live! After 3 days, I closed the form at 72 responses! 

<p class="centered-text">
<img class="centered" src="{{site.url}}/images/CSV.jpg" />
</p>

## Cleaning Responses


Cleaning responses were quite trivial. Using excel’s find and replace, all instances of “1-No” and “7-Yes” were replaced by 1 and 7 respectively. Using IP Addresses, duplicate entries were removed. 

Once imported into a cozy dataframe, I then had to clean up the hobbies part of user responses. The string responses were tokenized:

`

def hobbies_to_list(df):
    #hobbies_index = df.columns.get_loc("Hobbies")
    for i in range(0, participants):
        hobbies_as_string = df.at[i, "Hobbies"]
        hobbies_as_list = hobbies_as_string.split(', ')
        df.at[i, "Hobbies"] = hobbies_as_list
`

and we were all set to get started!

## Planning 

My intuitive thought was that I would plot all the user responses as vectors (with each dimension being a response), and pair up users that were closest together, as users with similar response sentiments would be the most compatible with one another. However, this would not take into account into hobby match compatibility, so other ideas were considered.

After researching the problem of how ideal matches could be produced, I came upon the Stable Roommate Problem. A solution to such a problem guarantees that given N people, each responding with a list N-1 preferred matches, pairs can be matched such that no two participants would rather both be with someone else than their assigned partner. On [Algorithma](https://algorithmia.com/algorithms/matching/StableRoommateAlgorithm/docs), I found an algorithm that bases itself on Robert Irvine’s solution. Such an algorithm takes in a dictionary of names, with each key being their list of length N-1 of perferred matches, and spits out ideal pairs! Perfect. 

<p class="centered-text">
<img class="centered" src="{{site.url}}/images/stable-roomates.jpg" />
</p>

All I needed now was to produce such a list of preferred matches for each participant. To do this I would need to come up with a compatibility rubric for each pair. 

## Compatibility Rating

Not wanting to play God too much, I looked around for a compatibility formula. [This](https://algorithmia.com/algorithms/matching/DatingAlgorithm) was found on Algorithma from a dating algorithm:

<p class="centered-text">
<img class="centered" src="{{site.url}}/images/algorithma.PNG" />
</p>


That looked like something I could base my compatibility rating calculation on! The only difference was that I was looking for compatible friends, not partners. I think that interests are more important than values, so I put more weight on interests than values. 

An example of one of the helpers that calculate compatibility. My goal here was to find all the similarities given two lists of hobbies:

```
Interest Compatibility Calculator
given 2 people, find the number of match hobbies, then calculate a rating

def hobby_similarity_calculate(p1,p2):
    hobby_multiplier = 25
    p1_copy = p1.copy()
    p2_copy = p2.copy()
    hobbies_matched = 0
    hobby_index = df.columns.get_loc("Hobbies")
    p1_hobby_list = p1_copy[hobby_index]
    p2_hobby_list = p2_copy[hobby_index]
    
    for p1_hobby in p1_hobby_list:
        for p2_hobby in p2_hobby_list:
            #print(f"Checking if {p1_hobby} is in {p2_hobby}")
            if p1_hobby in p2_hobby:
                if p1_hobby.isspace() or (not (p1_hobby.strip())): continue
                hobbies_matched += 1
                p1_hobby_list = [x for x in p1_hobby_list if x != p1_hobby] #trouble!
                
    for p2_hobby in p2_hobby_list:
        for p1_hobby in p1_hobby_list:
            if p2_hobby.isspace() or (not (p2_hobby.strip())):
                continue
            #print(f"Checking if {p2_hobby} is in {p1_hobby}")
                hobbies_matched += 1
                p2_hobby_list = [x for x in p2_hobby_list if x != p2_hobby]
            #else: print("no")
    rating_from_hobbies = hobby_multiplier * hobbies_matched
    return rating_from_hobbies

```

The code used to find matching hobbies was not as trivial as to find all matching strings! Consider these two hobbies lists

- [“league”, “playing striker in soccer”, “singing”]
- [“league of legends”, “soccer”, “sing”]


Even though they contain 3 of the same activities, such a trivial implementation would pick up on none. Hence, my implementation went through each string in the first list and looked if it existed in any of the strings in the second list. Here, “league” would show up in “league of legends”. It would then add one to hobbies matched, removed “league” from list one, and continue. Once list one was done, the same process would be repeated with list 2. This had to be repeated twice, because for eg, “singing” does not show up in “sing”, but the converse is true. 

Other alternatives I considered were to lemmatize the strings, however, that would not fix the problem described above (although it would correct match something like sing/singing). In the end, even though the approach was not the most computationally friendly, it sufficed for such a small dataset. 

One of the biggest headaches I had occurred here. Being new to pandas, I did not realize the difference of working with a copy of a dataframe and the original and got confused on why the dataframe kept changing. After that, I forgot that in python, altering a shallow list copy also alters the original! Also, the participants often ended off their list of hobbies with a comma followed by a space [..., ], so ' ' was interpreted to be a hobby. Thank goodness for Stack Overflow, but the hours I spent questioning where my code was going wrong were brutal.

The code used to produce similarity values:
```

def one_with_all_similarity(p, df):
    compatibility_with_p = {}
    p_id = p['ID']
    for potential_match in range(participants):
        participant_info = df.iloc[potential_match]
        pm_id = participant_info[df.columns.get_loc("ID")]
        if p_id != pm_id: # potential match cannot be yourself
            hobby_rating = hobby_similarity_calculate(p, participant_info)
            similarity_rating = similarity_calculate(p, participant_info)
            age_rating = age_similarity_calculate(p, participant_info)
            compatibility_with_p[pm_id] = similarity_rating + age_rating - hobby_rating
            
    sorting dictionary keys from least to greatest
    sorted_dict = {k: v for k, v in sorted(compatibility_with_p.items(), key=lambda item: item[1])}
    return sorted_dict
```

After having the list for one participant, I recursed the above function with all the participants and placed it into a dictionary. After putting the results through the Stable Roommate Algorithm, my list of matches was ready!

## Drafting and Sending Emails!
I drafted up a function that produced a dictionary, the key being the target email and the value being the message that would be sent:
```
emails_to_send = {}
for participant_ID, match_ID in SRA_result.items():
    
    participant_index = df.index[df['ID'] == participant_ID].tolist()[0]
    match_index = df.index[df['ID'] == match_ID].tolist()[0]
    participant_info = df.iloc[participant_index]
    match_info = df.iloc[match_index]
    
    participant_name = participant_info["Name"]
    participant_email = participant_info["Email"]
    match_name = match_info["Name"]
    match_hobbies = match_info["Hobbies"]
    match_hobbies_as_str = ", ".join(match_hobbies)
    match_email = match_info["Email"]
    message = f"""Hi {participant_name},
The data has been crunched through, and a friend has been found!
Their information is as follows:
    
Name: {match_name}
Hobbies: {match_hobbies_as_str}
Email: {match_email}
    
Your match has also been sent an email with your information. Whatever happens next is up to you guys!

Have fun and stay safe,
Operation Cordial"""
    
    emails_to_send[participant_email] = message
```

Then, all I needed was to find a script to automatically send out emails. This was easy enough, as with a bit of Googling and copy-pasting, I used functions from the smtplib library to send out all the emails:
```
import smtplib

for email, message in emails_to_send.items():
    with smtplib.SMTP('smtp.gmail.com', 587) as smtp:
        smtp.ehlo()
        smtp.starttls()
        smtp.ehlo()
        SENDER = 'operationcordial@gmail.com'
        reciever = email
        smtp.login('operationcordial@gmail.com', “PASSWORD’
        subject = "Your Operation Cordial Match!"
        body = message
        msg = f'Subject: {subject}\n\n{body}'
        smtp.sendmail(SENDER, reciever, msg)
```

## Response Analysis and Conclusion

The project was a success as all the emails were sent out! I promptly made sure to comment out the code sending the emails, so to not accidentally spam everyone!

When I first started, I had never used pandas, and only used Jupyter Notebook to a very limited extent. However, doing this project has allowed me to become a bit more confident in working with data in the future! Finishing this project gave me a sense of accomplishment, and it made me understand why I feel in love with the idea of coding in the first place. The potential to help so many people. After the project was run, I received heartwarming DMs saying how happy and thankful participants were for doing this. If I was able to whip this up in a week's time, I’m excited for what else is to come!

Something that could have been improved on was the crude compatibility calculations. Although it sufficed, a more in-depth implementation would have been to ask the users for how important they thought each value was to them, and base the compatibility values around that. 

Basic insights on the difference between how the males and females responded can be found on the Reddit followup post made [here](https://www.reddit.com/r/uwaterloo/comments/g6rj53/operation_cordial_participant_response_reflections/).
