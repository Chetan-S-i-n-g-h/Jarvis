# Jarvis
import random
import pyttsx3
import speech_recognition as sr
import datetime
import wikipedia
import webbrowser
import os
import smtplib

print(".........initializing Jarvis..........")
Master = "Boss "
engine = pyttsx3.init('sapi5')
voices = engine.getProperty('voices')
engine.setProperty('voice',voices[0].id)

#speak function will pronounce the string which is passed to it
def speak(text):
    engine.say(text)
    engine.runAndWait()

def wishMe():
    hour = int(datetime.datetime.now().hour)

    if hour>=0 and hour<12:
        speak("Good Morning" + Master)
    elif hour>=12 and hour<16:
        speak("Good After Noon " + Master)
    else:
        speak("Good Evening " + Master)

    speak("I am Jarvis, How may i help you Boss")


# Main Program starts here
def takeCommand():

    r = sr.Recognizer()
    with sr.Microphone() as source:
        print("Listening...")
        audio = r.listen(source)

    try:
        print("Recognizing...")
        query = r.recognize_google(audio,language="en-in")
        print("user said: " ,query)

    except Exception as e:
        print("boss , please say that again...")
        speak("Boss, please say that again...")
        query = None


    if query == None:
        takeCommand()

    return query

def sendEmail(to,content):
    server = smtplib.SMTP('smtp.gmail.com',587)
    server.ehlo()
    server.starttls()
    server.login('youremail@gmail.com','password')
    server.sendmail("chetuee@123.com",to,content)
    server.close()


#speak("initializing jarvis.....")
wishMe()
query =takeCommand()

#logic for executing the task as per query

if 'wikipedia' in query.lower():
    speak("Searching wikipedia...")
    query = query.replace("wikipedia","")
    results = wikipedia.summary(query,sentences=3)
    print(results)
    speak(results)

elif 'wish me happy birthday' in query.lower():
    speak("Happy Birthday Boss...")

elif 'open google' in query.lower():
    webbrowser.open("www.google.com")

elif 'open youtube' in query.lower():
    webbrowser.open("https://www.youtube.com")
    #url = "youtube.com"
    #chrome_path = 'https://www.youtube.com/'
    #webbrowser.get(chrome_path).open(url)

elif 'play music' in query.lower():
    url = {
    "0":"https://www.youtube.com/watch?v=RKioDWlajvo&list=PLMRKdK25AuPVu7vuwjDm2yobPuZS8hiiO",
    "1":"https://www.youtube.com/watch?v=XOVds2co8f8&list=PLl6h6UvLNv3-iuxKLFxgBDIJCYsq_B8gJ",#karan aujla
    "2":"https://www.youtube.com/watch?v=dQe6WftWjKY&list=RDEMx1euTuom-_RCkkm-PMgNJA&start_radio=1",#karan randhawa
    "3":"https://www.youtube.com/watch?v=RKioDWlajvo&list=PLaRMTrp7-DsbbFmBpvBs9XM42cfQ7OGaL",#jass manak
    "4":"https://www.youtube.com/watch?v=YPohR_9v6HA&list=PLs9szTeZFXAwd9YhcMbAjBKhZgkT_7Uzf",#hardy sandhu
    "5":"https://www.youtube.com/watch?v=HX8YBnmq5gU&list=PLFFyMei_d85XIZGAtpgX6SKyEqOmyGlSq"#top 50


    }
    url1 = url[str(random.randint(0,5))]

    webbrowser.open(url1)

elif 'play kya baat h' in query.lower():
    webbrowser.open("https://www.youtube.com/watch?v=G0Hx6uN2AJE")

elif ' play song' in query.lower():
    songs_dir = "C:\\X....Dhananjay\\Music"
    songs = os.listdir(songs_dir)
    print(songs)
    os.startfile(os.path.join(songs_dir,songs[0]))

elif ' the time' in query.lower():
    strTime = datetime.datetime.now().strftime("%H hour %M minutes %S seconds")
    speak(Master + " the time is" + strTime)
elif 'play haryanvi song' in query.lower():
    webbrowser.open("https://www.youtube.com/watch?v=p-mZAhTg7kg&t=11s")
elif 'email to' in query.lower():
    try:
        speak("What shouold i said")
        content = takeCommand()
        speak("confirm whom to send")
        to = takeCommand()
        sendEmail(to, content)
        speak("Email has been sent successfully")
    except Exception as e:
        print(e)
