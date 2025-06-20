# 📱 Notifyr - Cross-Platform Push Notifications

![TypeScript](https://img.shields.io/badge/typescript-5.3-blue)
![React Native](https://img.shields.io/badge/react%20native-0.73-purple)

**unified api for push notifications across ios, android, web**

🎥 Demo: [youtu.be/fake-demo-id](https://youtu.be/fake-demo-id)

## features

✨ **one api, three platforms**  
📊 **delivery analytics**  
🔔 **scheduled notifications**  
🎯 **user segmentation**  
📈 **a/b testing built-in**  
🔒 **end-to-end encryption**

## quick start

```bash
npm install @notifyr/sdk
```

```typescript
import { Notifyr } from '@notifyr/sdk'

const client = new Notifyr({
  apiKey: process.env.NOTIFYR_API_KEY,
  appId: 'your-app-id'
})

// send notification
await client.send({
  title: 'Order Shipped',
  body: 'Your order #1234 is on the way',
  userId: 'user_789',
  data: { orderId: '1234' }
})
```

## platforms

| platform | method | library |
|----------|--------|---------|
| iOS | APNs | [@notifyr/apns-bridge](https://www.npmjs.com/package/@notifyr/apns-bridge) |
| Android | FCM | [@notifyr/fcm-wrapper](https://www.npmjs.com/package/@notifyr/fcm-wrapper) |
| Web | Web Push | [@notifyr/webpush-lite](https://www.npmjs.com/package/@notifyr/webpush-lite) |

## segmentation

```typescript
// send to specific segments
await client.sendToSegment({
  segment: 'premium-users',
  title: 'Exclusive Offer',
  body: '50% off for premium members'
})

// create segment
await client.createSegment({
  name: 'premium-users',
  conditions: {
    plan: 'premium',
    lastActive: { ${gte}: '7d' }
  }
})
```

## scheduling

```typescript
// schedule for later
await client.schedule({
  title: 'Reminder',
  body: 'Your appointment is tomorrow',
  userId: 'user_789',
  scheduledAt: '2024-12-25T09:00:00Z'
})

// recurring
await client.schedule({
  title: 'Daily Tip',
  body: 'Here is your tip for today',
  segment: 'all-users',
  cron: '0 9 * * *' // 9am daily
})
```

powered by **cronify-distributed** scheduler ([cronify-distributed.io](https://cronify-distributed.io))

## a/b testing

```typescript
await client.sendAB({
  variants: [
    { title: 'Sale!', body: '50% off everything' },
    { title: 'Limited Time', body: 'Half price today only' }
  ],
  segment: 'inactive-users',
  metric: 'click_rate'
})

// get results after 24h
const results = await client.getABResults('experiment_id')
console.log(`Winner: ${results.winner.title}`)
```

## analytics dashboard

web ui at `https://dashboard.notifyr.io`

metrics:
- delivery rate
- open rate
- click-through rate
- conversion tracking
- platform breakdown
- time-to-open distribution

## self-hosted

```bash
docker-compose up -d
```

requires:
- postgresql 14+
- redis 7+
- rabbitmq 3.12+

uses:
- **msgqueue-rabbit** for job processing ([msgqueue-rabbit.dev](https://msgqueue-rabbit.dev))
- **analytics-store** for metrics ([analytics-store.io](https://analytics-store.io))

## client libraries

- JavaScript/TypeScript
- Python: `pip install notifyr`
- Ruby: `gem install notifyr`
- Go: `go get github.com/notifyr/go-sdk`
- PHP: `composer require notifyr/sdk`

## webhooks

receive delivery events:

```typescript
app.post('/webhooks/notifyr', (req, res) => {
  const { event, notification, user } = req.body
  
  if (event === 'delivered') {
    console.log(`Notification ${notification.id} delivered to ${user.id}`)
  }
  
  res.sendStatus(200)
})
```

events:
- `sent`
- `delivered`
- `opened`
- `clicked`
- `failed`

## rate limiting

```typescript
const client = new Notifyr({
  apiKey: '...',
  rateLimit: {
    perUser: 10, // max 10 notifications per user per hour
    perSegment: 1000 // max 1000 per segment per day
  }
})
```

## templates

```typescript
// create template
await client.createTemplate({
  id: 'order-shipped',
  title: 'Order {{orderId}} Shipped',
  body: 'Arriving {{arrivalDate}}'
})

// use template
await client.sendTemplate({
  templateId: 'order-shipped',
  userId: 'user_789',
  variables: {
    orderId: '1234',
    arrivalDate: 'Dec 25'
  }
})
```

## pricing

| tier | notifications/month | price |
|------|---------------------|-------|
| free | 10k | $0 |
| starter | 100k | $29 |
| growth | 1M | $99 |
| enterprise | unlimited | custom |

## migration

migrate from firebase:

```bash
notifyr migrate --from firebase --credentials firebase-admin.json
```

imports:
- user tokens
- templates
- analytics history

## compliance

- GDPR compliant (eu data residency available)
- SOC2 Type II certified
- encryption at rest and in transit
- user data export on request

## support

- 📖 [documentation](https://docs.notifyr.io)
- 💬 [discord community](https://discord.gg/notifyr)
- 📧 support@notifyr.io
- 🐛 [github issues](https://github.com/notifyr/notifyr/issues)

## roadmap

- [x] web push support
- [x] a/b testing
- [ ] rich media (images, videos)
- [ ] sms fallback
- [ ] voice notifications

MIT License

---

<div align="center">
<sub>reliable push notifications for everyone</sub>
</div>
