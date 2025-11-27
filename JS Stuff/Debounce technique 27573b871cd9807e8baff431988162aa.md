# Debounce technique

1. some types of events have the potential to fire rapidly, many times in a row (for example. "Mouse-move" and "scroll" event).
2. when handling such events, we must be careful not to do anything too time-consuming, or your handler will take up so much time that interaction with the document feels slow.
3. we can use `setTimeout()` to make sure you are not doing it too often. This is usually called debouncing event.
    - let's take an example of input event:
        1. we want to react when a user has typed something, but we don't want to do it immediately for every input event.
        2. when they are typing quickly we just want to wait until a pause occurs, instead of immediately performing an action, we set a timeout. We also clear the previous timeout (if any)