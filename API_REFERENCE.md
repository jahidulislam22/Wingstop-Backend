# API Reference

This middleware provides the following endpoints and integrations:

## Available Endpoints

### GET `/`
Returns API information and available endpoints.

### GET `/health`
Health check endpoint for monitoring.
- Returns: service status, uptime, environment info

### GET `/customers`
Fetches all customers from Rivo.
- Returns: List of Rivo customers with points balances

### GET `/points/:email`
Gets customer points balance by email.
- Params: `email` - Customer email address
- Returns: Customer data with points tally

### GET `/rewards`
Fetches all available rewards from Rivo.
- Returns: List of available rewards

### POST `/redeem-points`
Redeems customer points for a reward.
- Body: `{ email, rewardId, rewardName, points, credits }`
- Returns: Redemption details, discount code, updated balance

### POST `/notify-point-redemption`
Webhook endpoint for Rivo point redemption events.
- Receives webhook from Rivo when points are redeemed
- Sends email notification to customer with reward details

## External API Integrations

### Rivo Merchant API

**Base URL:** `https://developer-api.rivo.io/merchant_api/v1/`

**Authentication:** Direct API key in `Authorization` header (no "Bearer" prefix)

**Endpoints Used:**
- `GET /customers` - List all customers
- `GET /customers/{email}` - Get customer by email
- `GET /rewards` - List all rewards
- `POST /points_redemptions` - Redeem points for rewards

## Notes
- All Rivo API calls use direct API key (not Bearer token)
- Customer identifier can be email or Shopify customer ID
- Email notifications require SMTP configuration

