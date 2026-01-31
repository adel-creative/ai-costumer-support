# 💚 Starter Package - Quick Start Guide
## Get Running in 15 Minutes!

---

## ⚡ Prerequisites Checklist

Before starting, have these ready:
- [ ] **n8n account** ([sign up free](https://n8n.io))
- [ ] **OpenAI API key** ([get here](https://platform.openai.com/api-keys))
- [ ] **Email inbox** (Gmail recommended)
- [ ] **Qdrant account** ([free tier](https://cloud.qdrant.io))
- [ ] **Google account** (for tracking sheet)

**Time needed**: 15-20 minutes  
**Cost**: $0 setup fee | ~$0.0002 per email

---

## 🚀 5-Step Quick Setup

### Step 1: Import Workflow (2 min)

1. **Download**: Get `ai_support_starter.json`
2. **n8n**: Go to Workflows → **Import from File**
3. **Select**: Choose the downloaded file
4. **Import**: Click the button
5. ✅ **Done**: Workflow imported!

---

### Step 2: Connect Email (3 min)

**For Gmail**:
1. Enable IMAP: Settings → Forwarding and POP/IMAP → Enable IMAP
2. Create App Password:
   - Google Account → Security
   - 2-Step Verification → App passwords
   - Generate password for "Mail"
   - **Copy** the 16-character password

**In n8n**:
1. Open **Email Trigger** node
2. Click **Create New Credential**
3. Fill in:
   ```
   Email: support@yourcompany.com
   Password: [paste app password]
   IMAP Host: imap.gmail.com
   IMAP Port: 993
   ```
4. **Test Connection**
5. Set interval: **5 minutes**
6. ✅ **Save**

**For Outlook/Office365**:
```
IMAP Host: outlook.office365.com
IMAP Port: 993
Enable: Modern Auth in Office365 admin
```

---

### Step 3: Setup OpenAI (2 min)

1. Get API key from [OpenAI Platform](https://platform.openai.com/api-keys)
2. Open **AI Agent** node
3. Click **Create New Credential**
4. Paste API key
5. ✅ **Save**

---

### Step 4: Setup Qdrant Knowledge Base (5 min)

**Create Qdrant Collection**:
1. Sign up at [Qdrant Cloud](https://cloud.qdrant.io)
2. Create new cluster (Free tier OK)
3. Create collection:
   ```
   Name: starter_kb
   Vector size: 1536
   Distance: Cosine
   ```
4. Copy your API key

**Configure in n8n**:
1. Open **Knowledge Base** node
2. Add credentials:
   ```
   URL: https://xyz.cloud.qdrant.io
   API Key: [your key]
   Collection: starter_kb
   ```
3. ✅ **Save**

**Add Your First Documents**:

Create a simple FAQ file `faq.txt`:
```
Q: What are your business hours?
A: We're available Monday-Friday, 9 AM - 5 PM EST.

Q: How do I reset my password?
A: Go to Settings > Security > Reset Password. 
   Click the link and follow the instructions.

Q: What's your return policy?
A: 30-day money-back guarantee. 
   Email returns@company.com to start a return.

Q: How do I contact support?
A: Email support@company.com or use our live chat.

Q: Do you offer refunds?
A: Yes, within 30 days of purchase for any reason.
```

**Upload to Qdrant** (create separate workflow):
```
1. Read/Write Files node → Load faq.txt
2. Text Splitter → Chunk size: 200, overlap: 20
3. Embeddings OpenAI → text-embedding-3-small
4. Qdrant Insert → Collection: starter_kb
5. Execute once
```

---

### Step 5: Setup Tracking Sheet (3 min)

1. **Create Google Sheet**:
   - Go to [Google Sheets](https://sheets.google.com)
   - New Sheet: "AI Support Metrics"
   - Add headers in Row 1:
     ```
     Timestamp | Customer | Tokens | Cost
     ```

2. **Connect to n8n**:
   - Open **Log to Sheet** node
   - Click **Create New Credential**
   - **Authenticate** with Google
   - Select your sheet
   - Map columns:
     ```
     Timestamp → A
     Customer → B
     Tokens → C
     Cost → D
     ```
   - ✅ **Save**

---

## 🎯 Customize System Prompt (Optional, 2 min)

Make it sound like your brand:

**In AI Agent node**, update system message:
```
You are a helpful customer support assistant for [YOUR COMPANY NAME].

We are a [DESCRIBE YOUR BUSINESS - e.g., "SaaS platform helping teams collaborate"].

Your job:
1. Understand the customer's issue
2. Search our knowledge base for solutions
3. Provide clear, step-by-step answers

Our tone: [Choose: Friendly | Professional | Casual]
Response style: [Choose: Concise | Detailed | Conversational]

If you can't solve the issue, politely inform the customer 
to contact [YOUR EMAIL] or call [YOUR PHONE] for further assistance.

Always:
- Be empathetic and patient
- Use simple language
- Provide specific next steps
- Thank the customer
```

**Examples by industry**:

**E-commerce**:
```
You are a helpful shopping assistant for [STORE NAME].
We sell [PRODUCTS]. Be friendly and help customers with:
- Product information
- Order status
- Returns & exchanges
- Shipping questions
```

**SaaS**:
```
You are a technical support specialist for [PRODUCT NAME].
Help users with:
- Login issues
- Feature questions
- Bug reports
- Integration setup
Be technical but clear.
```

**Service Business**:
```
You are a booking assistant for [BUSINESS NAME].
Help customers with:
- Appointment scheduling
- Service information
- Pricing questions
- Location & hours
Be warm and welcoming.
```

---

## ✅ Testing (3 min)

### Send Test Email
1. **From another email**, send to your support inbox:
   ```
   Subject: Test - Password Reset
   
   Hi, I forgot my password and can't log in. 
   How do I reset it?
   ```

2. **Wait** 5 minutes (trigger interval)

3. **Check n8n executions**:
   - Go to Executions tab
   - Should see successful run
   - Review each node's output

4. **Check your email**:
   - Should receive AI response
   - Should reference knowledge base answer

5. **Check Google Sheet**:
   - New row added
   - Cost calculated

### Test Different Scenarios
```
Test 1: "What are your hours?"
→ Should find answer in KB

Test 2: "I need a refund"
→ Should explain return policy

Test 3: "Random gibberish asdkfjlaskdf"
→ Should politely say it can't help

Test 4: "Tell me a joke"
→ Should redirect to support
```

---

## 🎊 Activate & Monitor

### Activate Workflow
1. Click **Active** toggle (top right)
2. 🟢 Green = Active
3. Workflow now processing emails automatically!

### First 24 Hours Monitoring
- ✅ Check executions every few hours
- ✅ Review 2-3 actual responses
- ✅ Monitor Google Sheet for costs
- ✅ Note any issues or improvements

### Daily Routine (5 min)
1. **Morning**: Check Google Sheet
   - How many emails yesterday?
   - Total cost?
   - Any patterns?

2. **Review samples**: Pick 2 random responses
   - Quality good?
   - Tone appropriate?
   - Answers accurate?

3. **Update KB**: Notice recurring questions?
   - Add to faq.txt
   - Re-upload to Qdrant

---

## 💰 Cost Tracking

### Expected Costs
```
Volume/Day    Daily Cost    Monthly Cost
─────────────────────────────────────
10 emails     $0.21         $6.30
50 emails     $1.05         $31.50
100 emails    $2.10         $63.00
200 emails    $4.20         $126.00
```

### Monitor in Google Sheets
Add these formulas:

**Cell E1**: `Total Today`
**Cell E2**: `=SUMIF(A:A, TODAY(), D:D)`

**Cell F1**: `Avg Cost`
**Cell F2**: `=AVERAGE(D:D)`

**Cell G1**: `Volume Today`
**Cell G2**: `=COUNTIF(A:A, TODAY())`

---

## 🔍 Troubleshooting

### Email not triggering?
- ☑️ Check email credentials
- ☑️ Verify IMAP enabled
- ☑️ Test connection manually
- ☑️ Check interval setting (try 1 min)

### No response sent?
- ☑️ Check executions for errors
- ☑️ Verify OpenAI API key
- ☑️ Check OpenAI usage limits
- ☑️ Review email send credentials

### Poor answer quality?
- ☑️ Add more documents to KB
- ☑️ Make KB content more specific
- ☑️ Update system prompt
- ☑️ Test with different questions

### High costs?
- ☑️ Reduce max_tokens to 300
- ☑️ Shorter system prompt
- ☑️ Check for email loops
- ☑️ Review unusual queries

---

## 📈 Growing Beyond Starter

### When to Upgrade to Pro?

Upgrade when you need:
- ✅ **Multiple channels** (chat, WhatsApp)
- ✅ **Higher volume** (>200 emails/day)
- ✅ **Human review** for complex cases
- ✅ **Better AI** (GPT-4o vs mini)
- ✅ **Tool integrations** (order lookup, CRM)
- ✅ **Advanced analytics**

### Migration is Easy
1. Export your Qdrant collection
2. Import Pro workflow
3. Import your KB to new collection
4. Configure additional channels
5. Done! 🎉

---

## 🆘 Need Help?

### Resources
- 📚 **Full Documentation**: See `STARTER_DOCUMENTATION.md`
- 💬 **Community**: [n8n forum](https://community.n8n.io)
- 📧 **Email**: support@yourcompany.com

### Quick Tips
- 💡 Start small (10-20 documents)
- 💡 Test thoroughly before going live
- 💡 Update KB weekly
- 💡 Monitor costs daily
- 💡 Refine prompts based on feedback

---

## ✅ Quick Reference

### Typical Setup Times
```
Import workflow:        2 min
Email setup:           3 min
OpenAI setup:          2 min
Qdrant + KB:           5 min
Google Sheets:         3 min
Testing:               3 min
─────────────────────────────
Total:                18 min
```

### Cost Breakdown (per email)
```
GPT-4o-mini:    $0.00021
Embeddings:     $0.000001
Total:          $0.000211 (~$0.0002)
```

### Daily Checklist
```
☐ Check Google Sheet metrics
☐ Review 2-3 sample responses
☐ Note recurring questions
☐ Update KB if needed
☐ Monitor costs vs budget
```

---

## 🎉 You're Ready!

Congratulations! Your AI support agent is live.

**What happens now**:
1. 📧 Customers email support
2. 🤖 AI reads & understands
3. 🔍 Searches knowledge base
4. ✍️ Writes helpful response
5. 📤 Sends automatically
6. 📊 Logs everything

**Your role**:
- Monitor performance
- Improve knowledge base
- Review quality samples
- Track costs

**Expected results** (Week 1):
- ✅ 50-60% emails fully automated
- ✅ <60 second response times
- ✅ ~$2-4/day costs (100 emails)
- ✅ Happy customers! 😊

---

## 🚀 Next Steps

**Week 1**: Monitor & Learn
- Watch how AI handles queries
- Note what works well
- Identify gaps in knowledge

**Week 2**: Optimize
- Add more KB documents
- Refine system prompt
- Adjust based on feedback

**Week 3**: Scale
- Increase email volume
- Consider adding more workflows
- Evaluate upgrade to Pro

**Week 4**: Evaluate
- Review 30-day metrics
- Calculate ROI
- Plan next improvements

---

**Ready to launch?** Hit that **Active** button! 🚀

**Questions?** Check `STARTER_DOCUMENTATION.md` for details.

**Version**: 1.0.0  
**Package**: Starter 💚  
**Est. Setup Time**: 15-20 minutes