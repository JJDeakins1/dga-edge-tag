# Edge Tag Installation Guide

This guide provides step-by-step instructions for installing the Edge Tag tracking script on the DreamGiveaway website.

## Overview

The Edge Tag script enables event tracking and data collection for the DreamGiveaway website. This installation requires code to be added to both the `<head>` section and various page templates throughout the site.

---

## Step 1: Install Base Script in Head Section

Copy the code below and paste it into the `<head>` of the DreamGiveaway website (recommended: place it near the existing Facebook Pixel code).

```html
<script>
  window.edgetag=window.edgetag||function(){(edgetag.stubs=edgetag.stubs||[]).push(arguments)};
</script>
<script async type="text/javascript" src="https://duduf.dreamgiveaway.com/load"></script>

<script>
edgetag('init', {
    edgeURL: 'https://duduf.dreamgiveaway.com',
    disableConsentCheck: true
  })
</script>
```

---

## Step 2: Add Event Tracking Scripts

The following code snippets should be added to the body of the DreamGiveaway website. **All code snippets should be wrapped in `<script>` tags.**

### PageView Event

Add the below code to **every page** of the DreamGiveaway website:

```javascript
edgetag('tag', 'PageView')
```

### AddToCart Event

Add the below code to the cart page (`https://www.dreamgiveaway.com/cart`):

```javascript
edgetag('tag', 'AddToCart')
```

### InitiateCheckout Event

Add the below code to the checkout page (`https://www.dreamgiveaway.com/checkout`):

```javascript
edgetag('tag', 'InitiateCheckout')
```

### Purchase Event

Add the code below to the page template where `fbq('track', 'Purchase');` is used. The `value` and `currency` should be dynamically populated using the dataLayer.

```javascript
edgetag('tag', 'Purchase', { value: 10.0, currency: 'USD' })
```

### Donate Event

Add the code below to the page template where `fbq('track', 'Donate');` is used. The `value` and `currency` should be dynamically populated using the dataLayer.

```javascript
edgetag('tag', 'Donate', { value: 10.0, currency: 'USD' })
```

---

## Step 3: User Data Collection

To improve event match rate, dynamically populate the code below based on the details provided at `https://www.dreamgiveaway.com/checkout`.

**Additional details:** [Edge Tag User Data Documentation](https://app.edgetag.io/docs/js#user)

**Note:** All code snippets should be wrapped in `<script>` tags.

```javascript
edgetag('data', {
    email: 'user@domain.com',
    phone: '+1234567890',
    firstName: 'John',
    lastName: 'Doe',
    city: 'fremont',
    state: 'CA',
    country: 'US',
    zip: '12345',
  })
```

---

## Important Notes

- Ensure all JavaScript code snippets are wrapped in `<script>` tags when implementing
- Dynamic values (such as purchase amounts, user data) should be populated from your dataLayer or backend data
- The user data fields should be populated with actual user information from the checkout process
- Test all events after implementation to ensure proper tracking

---

## Related Documentation

- [Edge Tag JavaScript SDK Documentation](https://app.edgetag.io/docs/js#user)

