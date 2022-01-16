# This web application uses peak.js from BBC to segment video/audio.
User may load an audio & video and add segments indicating speakers IDs, no speech, or overlapping speech.

# Deployed website
http://speakersegmentation.herokuapp.com/

# Instructions
1. Load audio & video file corresponding to the same group.
2. Identify what speaker number to associate with each participant.
For example speaker 1 should be sitting at 11 o'clock, speaker 2 at 6 o'clock, and speaker 3 at 2 o'clock. Make sure you keep this consistent throughout the task!
3. Play the audio file - which will sync up with the video to help identify which speaker is speaking.
4. Click "Add segment: Speaker #" buttons to label who is speaking for that duration. You will see the segmentations at the bottom of the webpage. You can play each segment by clicking the "Play" button as well as remove them.
5. Make sure you click **"Download Results"** to save the file on your computer before closing the tab. It looks like the the data is still retained as long as you don't close the browser tab, but you'll certainly need to restart if you close the tab. You can also download your progress and save results to multiple files (do the first 10 minutes save to a file, close tab, reload everything and to the next 10 minutes and save to file). If you do this, make sure to note where you left off.
6. Rename the files to know which file it corresponded to (for example, g35.json).
