# Stripe Integration Setup Guide

## Overview
The Real Estate AI Platform now includes full Stripe integration for credit/token monetization using your Stripe account. This guide explains how to complete the setup.

## Your Stripe Configuration
- **Secret Key**: `sk_test_51ROPU8Fyj95atzDB8PeyNUFqKUYTclKgMk7qmhSAFmQ9Qd1eDE5y4Xp7oMlZgaPBGvWNXmf9yFi5BLdaeDMGxBAj00W8IYKcYy`
- **Publishable Key**: `pk_test_51ROPU8Fyj95atzDBVOgWzPek8Ib0sIbjnXoVr4CV3PW7cjdnDYtsdnkih8fEY6jWASYA2n3JyOl40WgQTTjdsMyL00tVSxc9KS`

## Required Stripe Dashboard Setup

### 1. Create Products and Prices
You need to create the following products in your Stripe Dashboard:

#### Starter Package
- **Product Name**: "Starter Package"
- **Price**: $99.00 USD
- **Billing**: Monthly recurring
- **Price ID**: `price_starter_monthly` (use this exact ID)

#### Professional Package  
- **Product Name**: "Professional Package"
- **Price**: $299.00 USD
- **Billing**: Monthly recurring
- **Price ID**: `price_professional_monthly` (use this exact ID)

#### Enterprise Package
- **Product Name**: "Enterprise Package" 
- **Price**: $999.00 USD
- **Billing**: Monthly recurring
- **Price ID**: `price_enterprise_monthly` (use this exact ID)

### 2. Set Up Webhooks
Create a webhook endpoint in Stripe Dashboard:
- **URL**: `https://yourdomain.com/api/webhooks/stripe`
- **Events to send**:
  - `invoice.payment_succeeded`
  - `customer.subscription.updated`
  - `customer.subscription.deleted`

### 3. Get Webhook Secret
After creating the webhook, copy the webhook signing secret and update the `endpoint_secret` in `src/services/stripe_service.py`.

## API Endpoints

### Credit Management
- `GET /api/credits/balance` - Get current credit balance
- `GET /api/credits/transactions` - Get transaction history
- `POST /api/credits/purchase` - Purchase credits (pay-as-you-go)
- `GET /api/credits/usage_summary` - Get monthly usage summary

### Subscription Management
- `POST /api/subscriptions/create` - Create recurring subscription
- `POST /api/subscriptions/cancel` - Cancel subscription
- `GET /api/pricing` - Get available pricing plans

### Webhooks
- `POST /api/webhooks/stripe` - Handle Stripe webhooks

## Pricing Structure

### Subscription Plans
1. **Starter Package**: $99/month, 1,000 credits
2. **Professional Package**: $299/month, 5,000 credits  
3. **Enterprise Package**: $999/month, unlimited credits

### Pay-as-You-Go
- **Rate**: $0.10 per credit
- No monthly commitment
- Credits never expire

### Credit Usage
- **Bot Creation**: 10 credits
- **QR Code Generation**: 2 credits
- **AI Conversation**: 1 credit per message
- **Data Training**: 5 credits per URL/document
- **Advanced Analytics**: 3 credits per report

## Frontend Integration

### Payment Method Collection
Use Stripe Elements to collect payment methods:

```javascript
// Example frontend code
const stripe = Stripe('pk_test_51ROPU8Fyj95atzDBVOgWzPek8Ib0sIbjnXoVr4CV3PW7cjdnDYtsdnkih8fEY6jWASYA2n3JyOl40WgQTTjdsMyL00tVSxc9KS');

// Create payment method
const {paymentMethod, error} = await stripe.createPaymentMethod({
  type: 'card',
  card: cardElement,
});

// Purchase credits
const response = await fetch('/api/credits/purchase', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Authorization': `Bearer ${accessToken}`
  },
  body: JSON.stringify({
    credit_amount: 100,
    payment_method_id: paymentMethod.id
  })
});
```

### Subscription Creation
```javascript
// Create subscription
const response = await fetch('/api/subscriptions/create', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Authorization': `Bearer ${accessToken}`
  },
  body: JSON.stringify({
    plan_type: 'professional',
    payment_method_id: paymentMethod.id
  })
});
```

## Security Considerations

1. **API Keys**: Your Stripe secret key is hardcoded for development. In production, use environment variables.

2. **Webhook Security**: The webhook endpoint verifies Stripe signatures to ensure authenticity.

3. **Payment Method Security**: Payment methods are stored securely in Stripe, not in your database.

4. **Credit Tracking**: All credit transactions are logged for transparency and auditing.

## Testing

### Test Cards
Use Stripe's test cards for development:
- **Success**: `4242424242424242`
- **Decline**: `4000000000000002`
- **3D Secure**: `4000002500003155`

### Test Webhooks
Use Stripe CLI to test webhooks locally:
```bash
stripe listen --forward-to localhost:5001/api/webhooks/stripe
```

## Production Deployment

1. **Environment Variables**: Set up production environment variables:
   ```
   STRIPE_SECRET_KEY=sk_live_...
   STRIPE_PUBLISHABLE_KEY=pk_live_...
   STRIPE_WEBHOOK_SECRET=whsec_...
   ```

2. **HTTPS**: Ensure your production domain uses HTTPS for Stripe integration.

3. **Webhook URL**: Update webhook URL to your production domain.

4. **Live Mode**: Switch to live Stripe keys and create live products/prices.

## Monitoring

Monitor your Stripe integration through:
- Stripe Dashboard for payments and subscriptions
- Application logs for webhook processing
- Database for credit transactions and balances

The integration is now complete and ready for testing with your Stripe account!

