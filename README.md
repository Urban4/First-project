# First-project
A trivia app simulation 
import sqlite3

conn = sqlite3.connect("Trivia.db")
cur = conn.cursor()
#print("connected!")
#---------------------------------------
global_name=[]
category_played = []
score = []
def user():
    global category_played
    global global_name
    global score
    global qualify
    # ---------------------------------------
    firstname = input("Enter your firstname Here: ")
    lastname = input("Enter your lastname Here: ")
    global_name = (lastname + " " + firstname)
    if not firstname.isalpha():
        print("ERROR! INVALID NAME. PLEASE ENTER LETTERS ONLY")
        return user()
    if not lastname.isalpha():
        print("ERROR! INVALID NAME. PLEASE ENTER LETTERS ONLY")
        return user()
    print(f"WELCOME!!!, {global_name} \nThis is the DUNE entertainments trivia game.\n"
          "This trivia will determine if you qualify for the live show.\n"
          "This game consists of multiple choice trivia quiz from 3 different categories.\n"
          "INSTRUCTIONS:\n"
          "1. select a category and answer the respective questions, each category has 1 question.\n"
          " NOTE: Once you select a category you cannot select from that same category again!\n"
          "2. when you are done with game you will be notified if you have qualified or not.\n"
          " NOTE: To qualify you have get at all3 questions correctly in each category."
          )
    input("Press ANY KEY to begin the game: ")
    for i in range (3):
        quiz()
    total()
    category_played = []
    score = []
# ---------------------------------------
def quiz():
    while True:
        print("----------------------------------------")
        print("Categories\nA. POLITICS \nB. SPORTS \nC. ENTERTAINMENT \n")
        select = input("select a category (A , B or C): ")
        select = select.upper()
        if select == "A":
            if 'politics' in category_played:
                print(
                    'This category has been selected before, SELECT ANOTHER CATEGORY')
                print("----------------------------------------")
                print("----------------------------------------")
                return quiz()
            return politics()
        if select == "B":
            if 'sports' in category_played:
                print(
                    'This category has been selected before, SELECT ANOTHER CATEGORY')
                print("----------------------------------------")
                print("----------------------------------------")
                return quiz()
            return sports()
        if select == "C":
            if 'entertainment' in category_played:
                print(
                    'This category has been selected before, SELECT ANOTHER CATEGORY')
                print("----------------------------------------")
                print("----------------------------------------")
                return quiz()
            return entertainment()
        if select not in ['A', 'B', 'C']:
            print('Please, select (A,B or C only)')
        else:
            break


#---------------------------------------
def entertainment():
    category_name = 'entertainment'
    if 'entertainment' in category_played:
        print('This category has been selected before, SELECT ANOTHER CATEGORY')
        print("----------------------------------------")
        print("----------------------------------------")
        return quiz()
    que = "select * FROM Entertainment order by RANDOM() "
    q = cur.execute(que).fetchone()
    print(q[1])
    #-------------------------------
    def answer():
        global entscore
        entscore = 0
        userans = input( "select (A,B,C OR D): ")
        userans =userans.upper()
        if userans not in ('A','B','C','D',):
            print ("invalid option")
            return answer()
        if userans == q[2]:
            print ("correct!")
            entscore +=1

        else:
            print(f"wrong! {q[2]}")
    answer()
    category_played.append(category_name)
#---------------------------------------
def politics():
    category_name = 'politics'
    if 'politics' in category_played:
        print(
            'This category has been selected before, SELECT ANOTHER CATEGORY')
        print("----------------------------------------")
        print("----------------------------------------")
        return quiz()
    que = "select * FROM Politics order by RANDOM() "
    q = cur.execute(que).fetchone()
    print(q[1])
    #-------------------------------
    def answer():
        global polscore
        polscore = 0
        userans = input( "select (A,B,C OR D): ")
        userans =userans.upper()
        if userans not in ('A','B','C','D',):
            print ("invalid option")
            return answer()
        if userans == q[2]:
            print ("correct!")
            polscore +=1

        else:
            print(f"wrong! {q[2]}")
    answer()
    category_played.append(category_name)
#---------------------------------------
def sports():
    category_name = 'sports'
    if 'sports' in category_played:
        print(
            'This category has been selected before, SELECT ANOTHER CATEGORY')
        print("----------------------------------------")
        print("----------------------------------------")
        return quiz()
    que = "select * FROM Sports order by RANDOM() "
    q = cur.execute(que).fetchone()
    print(q[1])
    #-------------------------------
    def answer():
        global sposcore
        sposcore = 0
        userans = input( "select (A,B,C OR D): ")
        userans =userans.upper()
        if userans not in ('A','B','C','D',):
            print ("invalid option")
            return answer()
        if userans == q[2]:
            print ("correct!")
            sposcore +=1

        else:
            print(f"wrong! {q[2]}")
    answer()
    category_played.append(category_name)

#---------------------------------------
def total():
    global totalscore
    totalscore = (sposcore+polscore+entscore)
    print (f"score: {totalscore}/3")
    if totalscore == 3:
        print("CONGRATULATIONS! You have qualified")
        f = open('QUALIFIERS.txt', 'a')
        f.write(global_name)
    else:
        print("Sorry, you didn't Qualify")

def next():

    print("Thank you for participating")
    nextp = input("press ANY KEY for the next participant: ")
    while True:
        if nextp.isspace or nextp.isalpha or nextp.isupper:
            return user()
user()
while 1:
    next()
