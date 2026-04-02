# Meeting-intelligence-hub-



import speech_recognition as sr
from docx import Document
from datetime import datetime

# Initialize recognizer
recognizer = sr.Recognizer()

# Create a document
doc = Document()
doc.add_heading('Meeting Transcript', 0)

# Use microphone
mic = sr.Microphone()

print("🎤 Listening... Press Ctrl+C to stop.\n")

try:
    with mic as source:
        recognizer.adjust_for_ambient_noise(source)

        while True:
            print("Listening...")
            audio = recognizer.listen(source)

            try:
                # Convert speech to text
                text = recognizer.recognize_google(audio)

                # Add timestamp
                time_stamp = datetime.now().strftime("%H:%M:%S")

                output = f"[{time_stamp}] {text}"
                print(output)

                # Save to document
                doc.add_paragraph(output)

            except sr.UnknownValueError:
                print("Could not understand audio")

            except sr.RequestError:
                print("API unavailable")

except KeyboardInterrupt:
    print("\nStopping... Saving document.")

    # Save file
    doc.save("meeting_transcript.docx")
    print("✅ Transcript saved!")