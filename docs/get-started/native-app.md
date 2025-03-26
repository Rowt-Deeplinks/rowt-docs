# Native App Configuration

When using Rowt in something like a React Native app, you'll need to lightly modify your iOS and Android configurations to include listeners for link intents.

# Intent Setup
> What is an intent and why do I need it?

An intent is some event that the OS emits that your app can use to trigger something.

In our case, 
- The intent listener in your native code triggers when a user opens a link
- Our SDK extracts the path (ex: /users/1234/post/abcd) to pass to JavaScript
- You pass the path to your `onLinkOpen` logic
- Your app navigates to the appropriate content

## iOS

iOS instructions

## Android

Android instructions

## onLinkOpen() Listener

In the root route of your app, subscribe to the event listener and assign logic that executes on link open.

```typescript
// Some logic here

```

