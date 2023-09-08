import cv2
import numpy as np
import pyttsx3
import speech_recognition as sr
import datetime
import os
import wikipedia
import webbrowser
import pyautogui

engine = pyttsx3.init('sapi5')
voices = engine.getProperty('voices')
engine.setProperty('voice', voices[1].id)

i = 0

def speak(audio):
    engine.say(audio)
    engine.runAndWait()

def commands():
    r = sr.Recognizer()
    with sr.Microphone() as source:
        print("listening...")
        r.pause_threshold = 1
        audio = r.listen(source)

    try:
        print("Wait for a few moments...")
        query = r.recognize_google(audio, language='en-in')
        print(f"You just said: {query}\n")
    except Exception as e:
        print(e)
        speak("Please tell me again")
        query = "none"

    return query

def wishings():
    hour = int(datetime.datetime.now().hour)
    if 0 <= hour < 12:
        print("Good morning...")
        speak("Good morning.")
        speak("This is Natasha AI assistant.")
        print("How can I help you?")
        speak("How can I help you?")
    elif 12 <= hour < 16:
        print("Good afternoon...")
        speak("Good afternoon.")
        speak("This is Natasha AI assistant.")
        print("How can I help you?")
        speak("How can I help you?")
    elif 16 <= hour < 19:
        print("Good evening...")
        speak("Good evening.")
        speak("This is Natasha AI assistant.")
        print("How can I help you?")
        speak("How can I help you?")
    else:
        print("Good night...")
        speak("Good night.")
        speak("This is Natasha AI assistant.")
        print("How can I help you?")
        speak("How can I help you?")

def me():
    me_text = "Hello sir. I am Natasha version 2.0, created by Kansha."
    speak(me_text)
    print(me_text)
    speak("How can I help you?")

def facedetection():
    speak("First, I have to detect your face to verify your identity.")
    print("First, I have to detect your face to verify your identity.")
    speak("Please wait for a few moments...")
    print("Detecting...")

def date():
    today = datetime.date.today()
    date_str = today.strftime("%d.%m.%Y")
    print(date_str)
    speak(date_str)

def TaskExecution():
    speak("Verification successful...")
    cam.release()
    cv2.destroyAllWindows()
    print("Verification successful...")
    speak("Welcome back, Kansha sir...")
    print("Welcome back, Kansha sir...")
    wishings()

    while True:
        query = commands().lower()
        if "wikipedia" in query:
            speak("Searching in Wikipedia")
            try:
                query = query.replace("wikipedia", "")
                results = wikipedia.summary(query, sentences=2)
                speak("According to Wikipedia")
                print(results)
                speak(results)
            except:
                print("No results found")
                speak("No results found")

        elif 'time' in query:
            strTime = datetime.datetime.now().strftime("%H:%M:%S")
            speak(f"Sir, the time is {strTime}")
            print(strTime)

        elif 'date' in query:
            date()

        elif 'exit program' in query or 'exit the program' in query or "you can leave now" in query:
            speak("I'm leaving, sir. Goodbye...")
            quit()

        elif "open pycharm" in query:
            speak("Opening PyCharm, sir")
            os.startfile("C:\\Program Files\\JetBrains\\PyCharm Community Edition 2021.3.3\\bin\\pycharm64.exe") #path of the app

        elif 'who is' in query:
            webbrowser.open(query)

        elif "open Google chrome" in query or "open chrome" in query or "open google" in query:
            speak("Opening Google Chrome, sir")
            os.startfile("C:\\Program Files\\Google\\Chrome\\Application\\chrome.exe")

        elif 'open youtube' in query:
            speak("What would you like to search for on YouTube?")
            s = commands()
            webbrowser.open(f"https://www.youtube.com/results?search_query={s}")

        elif "close chrome" in query or "exit chrome" in query or "exit google" in query or "close window" in query or "close this window" in query:
            pyautogui.hotkey('ctrl', 'w')
            speak("Closing Google Chrome, sir")

        elif "where is" in query:
            query = query.replace("where is", "")
            location = query
            webbrowser.open(f"https://www.google.com/maps/place/{location}")

        elif "weather" in query:
            speak("Please specify the city name.")
            city_name = commands()
            webbrowser.open(f"https://www.accuweather.com/en/in/{city_name}/189231/weather-forecast/189231")
            speak(f"Opening weather for {city_name}")

        elif "magic sentence" in query:
            # You can add your own functionality here.
            pass

        elif "what can you do for me" in query:
            speak("I can perform various tasks based on your voice commands.")

        elif "cool" in query or "nice" in query or "awesome" in query or "thank you" in query:
            speak("Thank you, sir!")

        elif "minimize" in query or 'minimize' in query:
            speak('Minimizing, sir')
            pyautogui.hotkey('win', 'down', 'down')

        elif "maximize" in query or 'maximize' in query:
            speak('Maximizing, sir')
            pyautogui.hotkey('win', 'up', 'up')

        elif "close the window" in query or 'close the application' in query:
            speak('Closing, sir')
            pyautogui.hotkey('ctrl', 'w')

        elif "say you love me" in query:
            speak("I can only speak the truth. I love you.")
            print("I can only speak the truth. I love you ❤️")

        elif "how much do you love me" in query:
            speak("Some things just aren't quantifiable.")

        elif "my name" in query:
            speak("Your name is Kansha.") #add ur name

        elif "who are you" in query or "who created you" in query or "what's your name" in query or "what is your name" in query:
            me()

if __name__ == '__main__':
    facedetection()
    recognizer = cv2.face.LBPHFaceRecognizer_create()
    recognizer.read('trainer.yml')
    cascadePath = 'haarcascade_frontalface_default.xml'
    faceCascade = cv2.CascadeClassifier(cascadePath)
    font = cv2.FONT_HERSHEY_SIMPLEX
    id = 2
    names = ['', 'kan']
    cam = cv2.VideoCapture(0, cv2.CAP_DSHOW)
    cam.set(3, 640)
    cam.set(4, 480)
    minW = 0.1 * cam.get(3)
    minH = 0.1 * cam.get(4)

    while True:
        ret, img = cam.read()
        converted_image = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
        faces = faceCascade.detectMultiScale(
            converted_image,
            scaleFactor=1.2,
            minNeighbors=6,
            minSize=(int(minW), int(minH)),
        )

        for (x, y, w, h) in faces:
            cv2.rectangle(img, (x, y), (x + w, y + h), (0, 255, 0), 2)
            id, accuracy = recognizer.predict(converted_image[y:y + h, x:x + w])

            if accuracy < 100:
                id = names[id]
                accuracy = f"  {round(100 - accuracy)}%"
                TaskExecution()
            else:
                id = "unknown"
                accuracy = f"  {round(100 - accuracy)}%"

            cv2.putText(img, str(id), (x + 5, y - 5), font, 1, (255, 255, 255), 2)
            cv2.putText(img, str(accuracy), (x + 5, y + h - 5), font, 1, (255, 255, 0), 1)

        cv2.imshow('camera', img)
        k = cv2.waitKey(10) & 0xff
        if k == 27:
            break

    print("Thanks for using this program, have a good day.")
    cam.release()
    cv2.destroyAllWindows()
