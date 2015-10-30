# Setup for the External Use Case session

## Installing Flask and PyJWT

-   Open a Command Prompt by clicking "Start", then typing `cmd`
-   In the Command Prompt, type `pip install flask pyjwt`

## Installing the "Python Tools for Visual Studio"

-   Open Visual Studio: Start > Type "Visual Studio", select “Visual Studio 2015”
-   If you are asked to register Visual Studio: Click "Not now, maybe later."
    -   If you are asked for your email, click the "X" used to close the window.
    -   Click "Start Visual Studio"
-   In the "Start" section, click "New Project&#x2026;"
-   A window will open with a box in the upper right labeled "Search
    Installed Templates", type "python" into that box.
-   You will see a search result show up named "Install Python
    Tools for Visual Studio". Select “Install Python Tools for Visual Studio”
-   Click the "OK" button, or press the
    "Enter" key.
-   A window titled "Install Missing Features" will open, click the
    "Install" button.
-   A dark window named "Setup Warnings" will open asking you to "Please
    close Visual Studio now &#x2026;". Close the main Visual Studio window that is
    in the background, then click "Retry" in the dark window named
    "Setup Warnings".
-   You should see the dialog with a checked box next to "Python
    Tools for Visual Studio". Click "Next"
-   Click the "UPDATE" button.
-   When you see "Setup Completed!", click the "Close" button.

## Write your first Flask application

-   Open Visual Studio again.
-   In the "Start" section, click "New Project"
-   Select "Python Application" from the list, then click the "OK"
    button (You can leave the name as "PythonApplication1")
-   From the "Project" menu, select "PythonApplication1 Properties"
-   In the "General" section, select the "Interpreter" that is
    labeled "Python 2.7", then click the "Save" (floppy disk) icon.
-   Switch back to the "PythonApplication1.py" window
-   Copy this code into the "PythonApplication1.py" window:
    
        from flask import Flask
        app = Flask(__name__)
        
        @app.route("/")
        def hello():
            return "Hello World!"
        
        if __name__ == "__main__":
            app.run()
    
    (Note: This code is from the "Flask is Fun" section of the
    [official web page for Flask](http://flask.pocoo.org/))
-   Click the "Start" button with the green "play" icon in it.
-   You should see a Command Prompt window open containing the text
    `Running on http://127.0.0.1:5000/` in it.
-   In your web browser, visit: <http://localhost:5000>
-   If you see the text "Hello World!" in it, then you've just
    written your first Flask app!