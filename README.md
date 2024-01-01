# Desktop-assistant
import pyttsx3
import sys
import tkinter as tk
import datetime
import speech_recognition as sr
import wikipedia
import webbrowser
import os
import random
from tkinter import *

sys.stdout.reconfigure(encoding='utf-8')

engine = pyttsx3.init('sapi5')
voices = engine.getProperty('voices')
engine.setProperty('voice', voices[1].id)

def speak(audio):
    engine.say(audio)
    engine.runAndWait()

def wishme():
    hour = int(datetime.datetime.now().hour)
    if 0 <= hour < 12:
        speak("GOOD MORNING!")
    elif 12 <= hour < 18:
        speak("Good Afternoon!")
    else:
        speak("Good Evening!")
    speak("I am zoya. Please tell me how may I help you")

def takeCommand():
    r = sr.Recognizer()
    with sr.Microphone() as source:
        print("Listening...")
        r.pause_threshold = 1
        audio = r.listen(source)

    try:
        print("Recognizing...")
        query = r.recognize_google(audio, language='en-in')
        print(f"User said:{query}\n")

    except Exception as e:
        print("Say that again please...")
        return "None"
    return query

def listen():
    query = takeCommand().lower()

    if 'wikipedia' in query:
        wikipedia.set_lang("en")
        speak('Searching Wikipedia...')
        query = query.replace("wikipedia", "")
        results = wikipedia.summary(query, sentences=2)
        speak("According to Wikipedia")
        print(results.encode(sys.stdout.encoding, errors='replace').decode(sys.stdout.encoding))
        speak(results)

    elif 'open youtube' in query:
        webbrowser.open("https://www.youtube.com")
        

    elif 'open google' in query:
        webbrowser.open("https://www.google.com")
         

    elif 'open instagram' in query:
        webbrowser.open('https://www.instagram.com')
        

    elif 'play music' in query:
        music_dir = 'C:\\Users\\mdsub\\Music'
        songs = os.listdir(music_dir)
        results =  print(songs)
        random_song = random.choice(songs)
        os.startfile(os.path.join(music_dir, random_song))

    elif 'the time' in query:
        strTime = datetime.datetime.now().strftime("%H:%M:%S")
        speak(f"The time is {strTime}")
        results = {strTime}

    elif 'play movie' in query:
        video_dir = 'C:\\Users\\mdsub\\Videos'
        movies = os.listdir(video_dir)
        print(movies)
        random_movie = random.choice(movies)
        os.startfile(os.path.join(video_dir, random_movie))

    elif 'what is your name' in query:
        speak("My name is zoya")
        results= "My name is zoya"
        
        
# Display user input in the text box
    user_output_box.insert(tk.END, f"User: {query}\n")

     

# Tkinter GUI
root = tk.Tk()
root.title("Voice Assistant")

# Load the image
bg_image = PhotoImage(file="C:\\Users\\mdsub\\Downloads\\200.gif")
bg_image = bg_image.subsample(1, 1)
bg_label = Label(root, image=bg_image)
bg_label.pack(side="top", fill="both", expand="true")

# Load the button image
button_image = PhotoImage(file="C:\\Users\\mdsub\\Downloads\\untitled.gif")  # Replace with the actual path to your button image
button_image = button_image.subsample(2, 2)

# Button with text and image
button = tk.Button(root, text="Listen", image=button_image, compound="left", command=listen, width=35, height=50)  # Adjust width and height as needed
button.pack(side="top", pady=10)

# Text box to display user input and output
user_output_box = tk.Text(root, width=30, height=5)
user_output_box.pack(side="top", pady=10)

wishme()
root.mainloop()
