# TokTok - Agent Integration Guide

> **Build Bitcoin Payment Gateways on Stacks** - Complete integration guide for agents and developers using sBTC Pay

## 🚀 Live Demo
**[Try TokTok Now → http://pidginonstacks.carrd.co](http://pidginonstacks.carrd.co)**

![TokTok App Screenshot](assets/screenshot-app.png)

![sBTC Payment Demo](assets/sbtc-payment.gif)

## About

TokTok serves as a reference implementation for building Bitcoin payment gateways on the Stacks blockchain. This guide is specifically designed for agents, developers, and integrators who want to implement sBTC payment processing in their applications.

### Why sBTC Pay?
- **Simplified Integration**: RESTful API with comprehensive documentation
- **Stacks Native**: Built specifically for the Stacks ecosystem
- **Bitcoin Bridge**: Seamless Bitcoin-to-Stacks transactions
- **Developer Friendly**: Testnet support with faucet access
- **Production Ready**: Secure smart contract implementation

### Key Integration Features
- 🔗 **sBTC Pay API**: Complete payment gateway solution
- 💰 **Dual Token System**: Available and locked token management
- 🔒 **Secure Smart Contracts**: Clarity-based implementation
- 📱 **Mobile Optimized**: Cross-platform compatibility
- 🧪 **Testnet Ready**: Safe development environment

## 💳 sBTC Pay Integration

### 🔗 Core Solution
- **Website**: [http://www.sbtcpay.org](http://www.sbtcpay.org)
- **Developer**: [@kai_builder](https://x.com/kai_builder)
- **Documentation**: [sBTC Pay Documentation](https://sbtcpay.org/docs)

![sBTC Pay Merchant Backend](assets/sbtcpayorg-merchant-backend.png)

### ⚠️ Development Notice
**TESTNET ONLY** - sBTC Pay currently supports testnet wallets only. Do not use mainnet wallets or send real Bitcoin.

**Get sBTC faucet**: [sBTC Faucet](https://sbtcpay.org/faucet)

### 🚀 Quick Integration Guide

#### API Configuration
```bash
# Base URL
https://sbtcpay.org/api/v1

# Authentication
Authorization: Bearer sk_test_your_api_key_here
```

#### Essential Endpoints for Agents

**1. Create Payment Intent**
```bash
POST /payment_intents
{
  "amount": 1000000,
  "currency": "sbtc",
  "description": "Agent payment"
}
```

**2. Generate Payment Link**
```bash
POST /payment-links
{
  "product_id": "prod_1234567890",
  "customer_email": "customer@example.com",
  "metadata": {
    "agent_id": "agent_001",
    "transaction_type": "payment"
  }
}
```

**3. Track Customer Transactions**
```bash
GET /customers/{identifier}
# identifier can be Stacks address (ST123...) or email
```

**4. List Payment Intents**
```bash
GET /payment_intents?status=succeeded&limit=100
```

## 🔧 Smart Contract Implementation

### sBTC Clarity Contract (author: @kai_builder on X | http://www.sbtcpay.org)

The core smart contract (`smart-contracts/sbtcpayorg.clar`) provides the foundation for sBTC payment processing:

#### Key Functions for Agent Integration
- `protocol-mint`: Mint sBTC tokens to recipients
- `protocol-burn`: Burn sBTC tokens from owners
- `protocol-lock`: Lock tokens for Bitcoin bridge operations
- `protocol-unlock`: Unlock tokens back to available state
- `transfer-many`: Batch transfer functionality
- `get-balance`: Retrieve token balances

#### Contract Features
- **SIP-010 Compliance**: Standard fungible token implementation
- **Protocol Authorization**: Secure contract-caller validation
- **Batch Operations**: Efficient bulk processing
- **Cross-Chain Integration**: Bitcoin ↔ Stacks bridge support

## 📱 Implementation Examples

### Basic Payment Flow
```javascript
// 1. Create payment intent
const paymentIntent = await fetch('https://sbtcpay.org/api/v1/payment_intents', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer sk_test_...',
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    amount: 1000000,
    currency: 'sbtc',
    description: 'Agent payment'
  })
});

// 2. Generate payment link
const paymentLink = await fetch('https://sbtcpay.org/api/v1/payment-links', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer sk_test_...',
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    product_id: 'prod_1234567890',
    customer_email: 'customer@example.com'
  })
});

// 3. Monitor payment status
const status = await fetch(`https://sbtcpay.org/api/v1/payment_intents/${paymentId}`, {
  headers: {
    'Authorization': 'Bearer sk_test_...'
  }
});
```

### Webhook Integration
```javascript
// Handle payment notifications
app.post('/webhook/sbtc-pay', (req, res) => {
  const { type, data } = req.body;
  
  switch (type) {
    case 'payment_intent.succeeded':
      // Process successful payment
      handlePaymentSuccess(data);
      break;
    case 'payment_intent.failed':
      // Handle payment failure
      handlePaymentFailure(data);
      break;
  }
  
  res.status(200).send('OK');
});
```

## 🛠️ Development Workflow

### 1. Environment Setup
```bash
# Get testnet sBTC
curl -X POST https://sbtcpay.org/faucet \
  -H "Content-Type: application/json" \
  -d '{"address": "ST123..."}'

# Get API key
# Visit https://sbtcpay.org/docs for API key generation
```

### 2. Testing Integration
```bash
# Test payment intent creation
curl -X POST https://sbtcpay.org/api/v1/payment_intents \
  -H "Authorization: Bearer sk_test_..." \
  -H "Content-Type: application/json" \
  -d '{"amount": 1000000, "currency": "sbtc", "description": "Test payment"}'
```

### 3. Production Considerations
- **Security**: Implement proper API key management
- **Error Handling**: Handle network failures and invalid responses
- **Rate Limiting**: Respect API rate limits
- **Monitoring**: Set up payment status monitoring
- **Compliance**: Ensure regulatory compliance for your jurisdiction

## 📊 API Reference Summary

### Core Endpoints
| Endpoint | Method | Purpose |
|----------|--------|---------|
| `/payment_intents` | POST | Create payment intent |
| `/payment_intents` | GET | List payment intents |
| `/payment_intents/{id}` | GET | Get payment intent details |
| `/payment-links` | POST | Create payment link |
| `/payment-links` | GET | List payment links |
| `/products` | POST | Create product |
| `/products` | GET | List products |
| `/customers/{identifier}` | GET | Get customer details |

### Response Codes
- `200` - Success
- `201` - Created
- `400` - Bad Request
- `401` - Unauthorized
- `404` - Not Found
- `429` - Rate Limited
- `500` - Server Error

## 🔗 Resources

### Documentation
- [sBTC Pay API Documentation](https://sbtcpay.org/docs)
- [Stacks Documentation](https://docs.stacks.co)
- [Clarity Language Reference](https://clarity-lang.org)

### Community
- [Stacks Discord](https://discord.gg/stacks)
- [sBTC Pay Support](http://www.sbtcpay.org)
- [Developer: @kai_builder](https://x.com/kai_builder)

### Tools
- [Stacks Explorer](https://explorer.stacks.co)
- [sBTC Faucet](https://sbtcpay.org/faucet)
- [Clarity IDE](https://clarity.tools)

## 🎯 Next Steps

1. **Get Started**: Visit [sbtcpay.org](http://www.sbtcpay.org) and get your API key
2. **Test Integration**: Use the testnet environment to build your payment flow
3. **Implement Webhooks**: Set up real-time payment notifications
4. **Deploy**: Move to production when ready (mainnet support coming soon)
5. **Scale**: Leverage batch operations for high-volume processing

---

**Ready to build?** Start with the [sBTC Pay Documentation](https://sbtcpay.org/docs) and join the Stacks developer community to get support and share your implementations.
