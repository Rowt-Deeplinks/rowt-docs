


## Use Rowt

If you want to get started fast, you can use Rowt's API service and connect your app with the SDK. 

In your React Native or Expo app:
```bash
npm install rowt-sdk
```


[Full SDK Docs](/app-sdk/)

[Create an account](https://console.rowt.app)

[Pricing](https://console.rowt.app/pricing)

<br>
<br>
<br>

## Self-Host
### Alternatively, you can self-host on your own infrastructure for more control over your API's behavior and costs.
We're an open source project, and this serves as a free or low cost way for us to make deep linking simpler :)

[Complete Self-Hosting Docs](/server)


<!-- # Console SDK

When deploying your own Rowt Server, the same API we use on the [Rowt Console](https://console.rowt.app) is accessible for anyone to use through our Console SDK. 

```bash
npm install rowt-console-sdk
```


```typescript
const sdk = new RowtConsole('https://your.server.url')

const user = await sdk.login('myserverusername', 'myserverpassword')

// Then you're free to use any method
// For example - Get all links and interactions for your project

const project = sdk.getProjectById('your-project-uuid')

```

[Complete Console SDK Docs](/console-sdk) -->


