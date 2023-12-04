### def update_score(username, user_score)
Updates database with the score and player name.
``` py
def update_score(username, user_score):

    """Updates the database with the score and player name.
       Adds the score to the latest scores list and boots the oldest score.
       If the score is higher than any of the scores in the high score leaderboard,
       boots the lowest score in the high score leaderboard and inserts the new score.
       Equal scores will prioritise the score submitted earlier on the high score leaderboard."""

    firebase = pyrebase.initialize_app(config)
    auth = firebase.auth()
    user = auth.sign_in_with_email_and_password(email, password)
    db = firebase.database()
    user = auth.refresh(user['refreshToken'])

    # Get the user's score from function call.
    # Encodes the user's name, score and time into a string.
    new_score = "{},{},{}".format(username, str(user_score), time())

    # Check if the user's score is worthy of the leaderboard, from the top down.
    for user_score_i in range(1, scoreboard_size + 1):
        node_hs = db.child(key_hs + str(user_score_i)).get(user['idToken'])
        # Scores in the database are saved in the format: highScoresi:username,user_score,time
        highscore_i = node_hs.val()

        # First checks if there is an entry in the current index, if there is, assign the score within to the variable.
        if highscore_i:
            highscore_i_value = int(highscore_i.split(",")[1])
        else:
            highscore_i_value = 0

        # The list is already sorted from highest to lowest, so if the user score is higher than an entry, all further
        # entries will be lower. If no such entry is found, the user score does not get added to the leaderboard.

        next_score = new_score
        if user_score > highscore_i_value:
            # Moves all entries down by one index. The last entry gets removed as it is not re-added to the database.
            for j in range(user_score_i, scoreboard_size + 1):
                node_hs_shift = db.child(key_hs + str(j)).get(user['idToken'])
                placeholder_score = node_hs_shift.val()
                db.child(key_hs + str(j)).set(next_score, user['idToken'])
                next_score = placeholder_score
            break

    # Resets the next_score variable to be the previously submitted score.
    next_score = new_score

    # Inserts the score into the list of the latest entries. Again, the last entry gets removed.
    for user_score_i in range(1, scoreboard_size + 1):
        node_ls = db.child(key_ls + str(user_score_i)).get(user['idToken'])
        placeholder_score = node_ls.val()
        db.child(key_ls + str(user_score_i)).set(next_score, user['idToken'])
        next_score = placeholder_score

```

### def fetch_high_scores()
Fetches high scores from database.
``` py
def fetch_high_scores():

    """Returns a list of lists of the 10 highest scores, from highest to lowest.
       e.g. [['hello', 2000, 1701536578.6759918], ['greetings', 1984, 1801536578.6759918]]"""

    firebase = pyrebase.initialize_app(config)
    auth = firebase.auth()
    user = auth.sign_in_with_email_and_password(email, password)
    db = firebase.database()
    user = auth.refresh(user['refreshToken'])

    highscore_list = []
    for list_index in range(1, scoreboard_size + 1):
        node_hs = db.child(key_hs + str(list_index)).get(user['idToken'])
        if node_hs.val():
            name, score, record_time = node_hs.val().split(",")
            # Variables exist in the database as encoded strings, so here we re-add their types.
            score = int(score)
            record_time = float(record_time)
            highscore_list.append([name, score, record_time])

    # Placeholder function to format list into string.
    highscore_list = leaderboard_format(highscore_list)

    return highscore_list

```

### def fetch_latest_scores()
Fetches most recent scores.
``` py
def fetch_latest_scores():

    """Returns a list of lists of the 10 latest scores, from latest to earliest.
       e.g. [['hello', 2000, 1701536578.6759918], ['greetings', 1984, 1801536578.6759918]]"""

    firebase = pyrebase.initialize_app(config)
    auth = firebase.auth()
    user = auth.sign_in_with_email_and_password(email, password)
    db = firebase.database()
    user = auth.refresh(user['refreshToken'])

    latest_scores_list = []
    for list_index in range(1, scoreboard_size + 1):
        node_ls = db.child(key_ls + str(list_index)).get(user['idToken'])
        if node_ls.val():
            name, score, record_time = node_ls.val().split(",")
            # Variables exist in the database as encoded strings, so here we re-add their types.
            score = int(score)
            record_time = float(record_time)
            latest_scores_list.append([name, score, record_time])

    # Placeholder function to format list into string.
    latest_scores_list = leaderboard_format(latest_scores_list)
    
    return latest_scores_list
```

### def leaderboard_format(score_list)
Formats leaderboard.
``` py
def leaderboard_format(score_list):

    """Takes in the score list from fetch_high_scores() or fetch_latest_scores()
       and returns a formatted string that fits into the set text box from 2048.py."""

    formatted_string = []
    for score in score_list:
        spaces = " " * (50 - len(score[0]) - len(str(score[1])))
        formatted_string.append(score[0] + spaces + str(score[1]))

    return formatted_string
```