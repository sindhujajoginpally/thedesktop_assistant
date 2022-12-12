# thedesktop_assistant

import pyttsx3
import speech_recognition as sr
import datetime
import wikipedia
import webbrowser
import os
import smtplib

print("Initializing raptor")

master = "Sindhuja"

engine = pyttsx3.init('sapi5')
voices = engine.getProperty('voices')
engine.setProperty('voice', voices[0].id)

#speak function will pronounce the string which is passes to it
def speak(text):
    engine.say(text)
    engine.runAndWait()

#this function will wish you as the per the given time
def wishMe():
    hour = int(datetime.datetime.now().hour)
    print(hour)

    if hour>=0 and hour <12:
        speak("Good Morning" + master) 

    elif hour>=12 and hour<18:
        speak("Good Afternoon" + master)  

    else:
        speak("Good Evening" + master)      

    speak("How can I help you?")

#this function wil take command from microphone
def takeCommand():
    r = sr.Recognizer()
    with sr.Microphone() as source:
        print("Listening...")
        audio = r.listen(source)

    try:
        print("recognizing...")
        query = r.recognize_google(audio, language = 'en-in')
        print(f"user said: {query}\n")

    except Exception as e:
        print("Say that again please")
        query = None  

    return query

def sendEmail(to, content):
    server = smtplib.SMTP('smtp.gmail.com', 587)
    server.ehlo()
    server.starttls()
    server.login('sinjogin@gmail.com', 'Sindhuja*1701')
    server.sendmail("prakhyaath.bittu@gmail.com", to, content)
    server.close()


# main program starts
speak("Initializing raptor") 
wishMe() 
query = takeCommand()

#logic for executing tasks as per the query
if 'wikipedia' in query.lower():
    speak('Searching wikipedia...')
    query=query.replace("wikipedia", "")
    result = wikipedia.summary(query, sentences =2)
    print(result)
    speak(result)

elif 'open youtube' in query.lower():
    #webbrowser.open("youtube.com")
    #url = "youtube.com"
    webbrowser.open('http://www.youtube.com')
    #chrome_path = 'C:/Program Files (x86)/Google/Chrome/Application/chrome.exe %s'
    #webbrowser.get(chrome_path).open(url, new=new)

elif 'open google' in query.lower():
   
    webbrowser.open('http://www.google.com')

elif 'open whatsapp' in query.lower():
   
    webbrowser.open('https://web.whatsapp.com/')
