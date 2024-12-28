## Diagram of flow of events (when submitting a form) for the non-SPA page

sequenceDiagram
participant Browser
participant Server

    Note over Browser: A) Note page contains a Form element
    Note over Browser: B) User clicks Save button (submits form)
    Note over Browser: C) HTTP requests are generated (look at parallel section)
    Note over Browser: D) User input sent to server at /new_note
    Note over Browser: E) Sever creates a new note and <br/>instruct the browser to reload the Notes page
    Note over Browser: F) Browser makes 4 requests at once: <br/> 1. Gets main page content <br/> 2. apply css styling <br/> 3. Downloads JS file <br/> 4. Fetches updates list of notes

    Browser->>+Server: 1. POST /new_note<br/>Form data: {note: content}

    Note over Server: Create new note object<br/>Add to notes array
    Server-->>-Browser: 2. HTTP POST 302<br/>URL redirect to /notes

    Browser->>+Server: 3. HTTP GET request to (header's location) /notes
    Server-->>-Browser: HTML content


    par Parallel requests
        Browser->>+Server: 4. GET main.css
        Server-->>-Browser: CSS styles
        Browser->>+Server: 4. GET main.js
        Server-->>-Browser: JavaScript code
        Browser->>+Server: 4. GET data.json
        Server-->>-Browser: Notes data in JSON format
    end

    Note over Browser: Page reloaded with<br/>new note displayed

## Some thoughts and conclusions

It was interesting to observe how the change in technology and approaches drastically influences the landscape for the end user.
In the traditional approach, we see:

In the **traditional architecture**:

- multiple HTTP requests (5 total)
- Server tasked with handling page updates and redirects
- Complete page reload is necessary
- Server is a key player in the architecture

The architecture described above works without JS and has a simpler server-side logic. However, requires more server load and it's slower and overall a not-so-great user experience.
