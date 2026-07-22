# n8n E-Commerce Automation System

> Fully automates the post-order process — inventory checks, low stock alerts, customer confirmations, and invoice generation — triggered instantly when a new WooCommerce order is placed.

## 🎯 Use Case
E-commerce stores lose time and make mistakes when order fulfilment is handled manually. Staff have to check inventory, email customers, generate invoices, and alert the warehouse — all for every single order. This workflow handles the entire post-order sequence automatically: the moment a WooCommerce order is placed, n8n checks inventory levels, sends a branded confirmation email to the customer, generates and attaches a PDF invoice, and alerts the warehouse team via Slack. If stock drops below a defined threshold, a separate low-stock alert is fired to the purchasing team.

## 🔄 Workflow Overview
1. **Trigger:** WooCommerce webhook fires on every new order placed.
2. **Inventory Check:** An HTTP Request node calls the WooCommerce Products API to get current stock levels for each ordered item.
3. **Branch A — Low Stock Alert:** If any item stock falls below the threshold (default: 10 units), a Slack alert is sent to the purchasing team with the product name and remaining quantity.
4. **Branch B — Customer Confirmation:** A branded HTML confirmation email is sent to the customer via SendGrid with their order summary.
5. **Invoice Generation:** An HTTP Request node calls a PDF generation API (PDFMonkey or similar) to produce a formatted invoice.
6. **Email Invoice:** The generated invoice PDF is attached to a second email and sent to the customer.
7. **Warehouse Notification:** A Slack message is posted to the fulfilment channel with order details and a packing checklist.
8. **Log Order:** All order data and automation outcomes are appended to a Google Sheet for reporting.

## 🛠️ Nodes Used
- WooCommerce Trigger (Webhook)
- HTTP Request (Inventory check + PDF generation)
- IF / Switch (Stock threshold routing)
- SendGrid (Customer confirmation + Invoice email)
- Slack (Low stock alert + Warehouse notification)
- Google Sheets (Order logging)

## ⚙️ Setup
1. Import `workflow.json` into your n8n instance (Workflows → Import from File).
2. Copy `.env.example` to `.env` and fill in your credentials.
3. In WooCommerce admin, go to Settings → Advanced → Webhooks and add a new webhook pointing to this workflow's n8n webhook URL, triggered on "Order Created".
4. Connect SendGrid, Slack, and Google Sheets inside n8n's credential manager.
5. Update the `LOW_STOCK_THRESHOLD` in your `.env` to match your business requirement.
6. Activate the workflow.

## 📋 Requirements
- n8n v1.0+
- WooCommerce store with REST API enabled
- WooCommerce Consumer Key and Consumer Secret
- SendGrid account with a verified sender email
- Slack workspace with bot token
- Google account with Sheets API enabled
- PDFMonkey (or similar) account for invoice generation

## 📂 Project Structure
```
n8n-ecommerce-automation/
├── README.md
├── workflow.json
├── .env.example
├── docs/
│   └── architecture.md
└── example-payloads/
    └── order-webhook-sample.json
```

## 🚀 Future Improvements
- Add order status update webhook (trigger on fulfilment, not just creation)
- Integrate with Royal Mail or DHL API to auto-book shipment and send tracking number
- Add a Telegram notification channel for mobile alerts
- Support multi-currency invoices for international orders
