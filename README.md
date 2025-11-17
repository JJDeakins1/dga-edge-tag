# Edge Tag Installation Guide

This guide provides step-by-step instructions for installing the Edge Tag tracking script on the DreamGiveaway website.

## Overview

The Edge Tag script enables event tracking and data collection for the DreamGiveaway website. This installation requires code to be added to both the `<head>` section and various page templates throughout the site.

---

## Overview

### Edge Tag Technical Requirements

The implementation of Edge Tag requires the following:

1. **Inline code modification to the Dream Giveaway website** - PENDING
   - New code snippets that initialize Edge Tag and route event data through Edge Tag and onto Meta via the conversion API

2. **Additional DNS records** - DONE
   - DNS records need to be added to the Dream Giveaway domain registrar (Amazon Route 53)

### Implementation Approach

The code modifications include new code snippets that initialize Edge Tag and route event data through Edge Tag and onto Meta via the conversion API.

**Important:** These code snippets would replace the Facebook code snippets currently found on the website. However, we recommend implementing this in a **phased approach**:

#### Phase 1: Testing
1. Install Edge Tag code snippets and test the configuration with a new testing pixel
2. Verify all events are tracking correctly
3. Monitor event data and match rates

#### Phase 2: Production Migration
1. Connect Edge Tag to the production pixel (`881584088553063`)
2. Remove inline Meta code (Facebook Pixel code)
3. Monitor performance and ensure no disruption to tracking

This phased approach minimizes risk and allows for thorough testing before migrating the production pixel.

---

# Phase 1

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

## Step 4: Wicked Reports Tracking Script
Copy the code below and paste it into the `<head>` of the DreamGiveaway website (recommended: place it near the existing Facebook Pixel code).
```javascript
<script type="text/javascript" src="https://widget.wickedreports.com/v2/5193/wr-9f67cf83a3fa2fa482d75f02480be6cc.js" async></script>
```


---

## Important Notes

- Ensure all JavaScript code snippets are wrapped in `<script>` tags when implementing
- Dynamic values (such as purchase amounts, user data) should be populated from your dataLayer or backend data
- The user data fields should be populated with actual user information from the checkout process

---

## Related Documentation

- [Edge Tag JavaScript SDK Documentation](https://app.edgetag.io/docs/)

---

# Phase 2: Next Steps

Once Phase 1 testing is complete and all events are verified to be tracking correctly, proceed with the following steps for production migration:

1. **Connect Edge Tag to Production Pixel** - Tier 11
   - Update Edge Tag configuration to connect to production pixel (`881584088553063`)
   - Verify connection is established and events are flowing to the production pixel

2. **Remove Inline Meta Code** - DreamGiveaway
   - Remove existing Facebook Pixel inline code from the website

3. **Monitor Performance** - Tier 11
   - Monitor event tracking in Meta Events Manager
   - Verify event match rates and data quality
   - Ensure no disruption to existing campaigns or reporting

4. **Post-Migration Verification** - Tier 11
   - Confirm all events (PageView, AddToCart, InitiateCheckout, Purchase, Donate) are tracking correctly
   - Verify conversion API events are being received by Meta
   - Check that user data is being properly matched and attributed

