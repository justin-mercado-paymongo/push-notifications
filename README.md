# Push Notifications

Push notifications are clickable messages that pop up on your phone or desktop with some content. 

## How Push Notifications Happen

![alt text](./Screenshot%202024-03-18%20at%2010.12.32 AM.png)

1. The client logs into our app and allows notifications
1. Client makes a request to our backend to save the device ID.
1. Depending on our chosen approach;
  - We can store the device ID from the client directly
  - We send the device ID to a 3rd party service where they map it to an ID unique to their service. We store the ID sent by the 3rd party service and use that to identify users by device

![alt text](./Screenshot%202024-03-17%20at%203.56.12 AM.png)

1. A transaction happens
1. PayMongo backend prepares the notification data (template options, device ID, description, title)
1. Push Service receives notification request, it prepares it's own request.
1. Request gets sent to Apple and/or Google servers
1. Client receives request



## Two approaches to push-notifications

1. Building an Inhouse Push Service
1. Third-Party Push Notification Service Providers

### Inhouse Push Service

Will require a whole separate service to be built solely for managing push notifications. This includes: managing user details and their offline and online status, routing the requests to the proper channels (Apple services, Android services), handling errors, storing push history, etc.

#### Cons

1. Dev costs to implement service
1. Maintenance costs
    - Possible downtime if google or apple update their APIs
    - More dev time when google or apple update their APIs

#### Pros

1. There will be a point where with enough "Push Notifications" having our own implementation will be a lot cheaper.

### Third-Party Push Service Providers

[OneSignal](http://onesignal.com/)
[Google Firebase](https://firebase.google.com/docs/cloud-messaging)
[Amazon SNS](https://aws.amazon.com/sns/?whats-new-cards.sort-by=item.additionalFields.postDateTime&whats-new-cards.sort-order=desc)
[Pusher](https://pusher.com/)
[Braze](https://www.braze.com/)

Provides all of the required 

#### Cons

1. Less flexibility - It is possible where we will require a feature that is not provided by the service.
1. Uncertainty of service quality - If the service we choose suddenly does something detrimental on our end, we will need to put up with it or dedicate extra dev time to migrate services

#### Pros

1. They handle all of the details. If Apple or Google update their APIs it would be in their best interest to keep their services updated
1. Have handled all possible situations we might not have considered when it comes to sending push notifications

### Affected internal APIs

`Core`, `Links`, `Pages` and `QRPh` will all need updates to send notifications to the Push service

DB Schema updates to accomodate storage of device IDs.

### Timeline Estimate

![alt text](./Screenshot%202024-03-17%20at%203.51.18 AM.png)

**INITIAL ESTIMATE** - Estimates can change once specifics are discussed

1. Phase 1 - Experimentation. Creating mock APIs. Documenting and finding out what teams and services need to be looped. Creating detailed plans for implementation
1. Phase 2 - Working with the outsourced mobile team for implementation details. Finding out what they need on the API end. Documenting details
1. Phase 3 - Working with internal teams to apply any changes needed to get push notifications. Also looping in mobile for possible implementation changes on the mobile side. Documenting details
1. Phase 4 - Manual and automated testing. Implementing fixes to issues found after in-depth testing. Applying last minute changes/improvements.

## Considerations

1. Our services need to have a way to store device IDs that will be associated with users

    - User may be logged into multiple devices, storing data other than ID (iOS/Android version) 

    - Depending on what approach we choose, mapping a user and ID will vary.

    - Third party services have planned out all of the "paths" when pushing notifications to users including logging out, authentication between their servers, our servers, Apple/Google servers and client SDK

2. A Push Notification Service handles the specific logic on what notification to send, where to send it, and some 3rd party service have their own device ID mapping. 

3. The approach we choose will also affect how the client SDK will be developed

## Resources

[Smashing Magazine - The Ultimate Guide To Push Notifications for Developers](https://www.smashingmagazine.com/2022/04/guide-push-notifications-developers/)

[Huspi - Everything You Need to Know About Push Notifications](https://huspi.com/blog-open/what-is-a-push-notification-and-how-does-it-work/)