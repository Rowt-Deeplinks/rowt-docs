# Getting Started

## Use Our Servers

If you're looking to get started fast, you can use the Rowt service and connect your app with the SDK. [Learn more about our service here.](https://console.rowt.app)

## Deploy your own Rowt Server

For more control over behavior and costs, you can spin up a Rowt Server on your own infrastructure. We're an open source project, and this serves as a free or low cost way for us to make deep linking simpler.

[Complete Server Docs](/server)

<br>
<br>
<br>

# Console SDK

When deploying your own Rowt Server, the same API we use on the [Rowt Console](https://console.rowt.app) is accessible for anyone to use through our Console SDK. 

```bash
npm install rowt-console-sdk
```


```typescript
const sdk = new RowtConsole('https://your.server.url')

sdk.login('myserverusername', 'myserverpassword')

// Then you're free to use any method
// For example - Get all links and interactions for your project

sdk.getProjectById

```

[Complete Console SDK Docs](/console-sdk)


