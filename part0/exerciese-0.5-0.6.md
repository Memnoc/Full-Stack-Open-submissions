## Diagram of flow of events (when submitting a form) for the SPA page

sequenceDiagram
participant Browser
participant Server

    Note over Browser: A) Note page contains Form element<br/>(no action/method attributes)
    Note over Browser: B) User clicks Save button
    Note over Browser: C) JavaScript prevents default form submission (e.preventDefault())
    Note over Browser: D) JS creates note object and<br/>updates page immediately

    Browser->>+Server: POST /new_note_spa<br/>Content-Type: application/json<br/>{content: "note", date: timestamp}
    Server-->>-Browser: 201 Created

    Note over Browser: E) Page already updated,<br/>no reload needed

## Some thoughts and conclusions

It was interesting to observe how the change in technology and approaches drastically influences the landscape for the end user.
In the traditional approach, we see:

In the **SPA (Single Page Application)**:
This is a end-user-centric approach, where the focus is on good user experience, and the technology abstracts the logic to the client, away from the server, to make sure the
re-loading are kept to a minimum and the data is present when the user interacts with the page.

- Single HTTP request (4 less than the previous approach)
- Browser API handles all the UI updates
- No page reloads are needed
- client-side logic (almost no server involved)

However, this approach is very JS-heavy, which means introducing more code that "could" fail and crash the application. The client-side code gets increasingly more complex and it may take a long time to load the page at first.

It is also really interesting to reflect on some of the most recent trends going in the opposite direction, like HTMX and SSR with Next.js.
