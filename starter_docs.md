# 💚 AI Customer Support - Starter Package
## Complete Documentation

---

## 📋 Table of Contents
1. [Overview](#overview)
2. [Architecture](#architecture)
3. [Setup Guide](#setup-guide)
4. [Configuration](#configuration)
5. [Usage](#usage)
6. [Cost Analysis](#cost-analysis)
7. [Troubleshooting](#troubleshooting)
8. [FAQ](#faq)

---

## 🎯 Overview

### What is Starter Package?
The Starter Package is a **budget-friendly AI customer support solution** designed for small teams and startups. It provides automated email support using AI at a fraction of the cost of traditional support.

### Key Features
- ✅ **Email-only support** - Simple, focused automation
- ✅ **GPT-4o-mini AI** - Cost-optimized intelligence
- ✅ **Knowledge Base (RAG)** - Search your documentation
- ✅ **Auto-response** - No manual intervention needed
- ✅ **Basic analytics** - Track performance & costs
- ✅ **Google Sheets logging** - Easy monitoring

### Best For
- 🏢 Startups with 1-10 employees
- 💰 Budget-conscious teams
- 📧 Email-only support needs
- 📊 Low volume (<200 emails/day)
- 🧪 Testing AI support before scaling

### Not Suitable For
- ❌ Multi-channel support (use Pro)
- ❌ Complex escalations (use Pro)
- ❌ High volume >200/day (use Enterprise)
- ❌ Real-time chat (use Pro/Enterprise)

---

## 🏗️ Architecture

### Workflow Overview
```
📧 Email → Context Prep → AI Agent → Response → Logging → Sheet
           ↓              ↓                      ↓
        Customer       Knowledge              Cost
         Info           Base                Tracking
```

### Components

#### 1. Email Trigger
- **Type**: IMAP/Email Trigger
- **Purpose**: Monitors support inbox
- **Checks**: Every 1-5 minutes
- **Filters**: Can filter by subject/sender

#### 2. Context Preparation
- **Extracts**: Customer email, subject, body
- **Formats**: Simple JSON structure
- **No**: Historical data, sentiment, priority

#### 3. AI Agent (GPT-4o-mini)
- **Model**: gpt-4o-mini
- **Cost**: ~5x cheaper than GPT-4o
- **Speed**: Fast responses (<10s)
- **Tools**: Knowledge base only
- **Memory**: Session-based only

#### 4. Knowledge Base
- **Technology**: Qdrant Vector Store
- **Embeddings**: text-embedding-3-small
- **Capacity**: Up to 10,000 documents
- **Search**: Top 3 relevant results

#### 5. Response System
- **Method**: Email auto-send
- **No**: Human review or approval
- **Format**: Plain text or HTML
- **Signature**: Auto-appended

#### 6. Analytics
- **Storage**: Google Sheets
- **Metrics**: Timestamp, customer, cost, tokens
- **Updates**: Real-time append
- **Retention**: Unlimited (in Sheets)

---

## 🚀 Setup Guide

### Prerequisites
Before starting, ensure you have:
- ✅ n8n instance (cloud or self-hosted)
- ✅ OpenAI API key ([get here](https://platform.openai.com/api-keys))
- ✅ Support email inbox (Gmail/Outlook/etc)
- ✅ Qdrant instance ([free cloud tier](https://cloud.qdrant.io/))
- ✅ Google account (for Sheets)

### Step 1: Import Workflow
1. Download `ai_support_starter.json`
2. In n8n: **Workflows → Import from File**
3. Select the JSON file
4. Click **Import**

### Step 2: Configure Email Trigger
1. Open **Email Trigger** node
2. Click **Create New Credential**
3. Enter your email settings:
   ```
   Email: support@yourcompany.com
   Password: [app-specific password]
   IMAP Host: imap.gmail.com
   IMAP Port: 993
   ```
4. Test connection
5. Set **Check Interval**: 5 minutes (or less)

### Step 3: Setup OpenAI
1. Open **AI Agent** node
2. Click **Create New Credential**
3. Enter your OpenAI API key
4. Save

### Step 4: Configure Qdrant
1. Open **Knowledge Base** node
2. Enter Qdrant credentials:
   ```
   URL: https://xyz.cloud.qdrant.io
   API Key: [your-api-key]
   Collection: starter_kb
   ```
3. Save

### Step 5: Prepare Knowledge Base
Create a new workflow to populate your KB:
```json
1. Read Files node → Load your docs (PDF, TXT, MD)
2. Text Splitter → Chunk into segments
3. Embeddings → Generate vectors
4. Qdrant Insert → Store in collection "starter_kb"
```

Example documents to include:
- Product FAQ
- How-to guides
- Common troubleshooting steps
- Return/refund policies
- Contact information

### Step 6: Configure Email Sending
1. Open **Send Response** node
2. Setup email credentials (same as trigger or different)
3. Customize email template if needed
4. Test with a dummy email

### Step 7: Setup Google Sheets
1. Create a new Google Sheet named "AI Support Metrics"
2. Add headers: `Timestamp | Customer | Tokens | Cost`
3. Open **Log to Sheet** node
4. Authenticate with Google
5. Select your sheet
6. Map columns correctly

### Step 8: Customize System Prompt
Edit the AI Agent's system message:
```
You are a helpful customer support assistant for [YOUR COMPANY].

Your job:
1. Understand the customer's issue
2. Search the knowledge base for solutions
3. Provide clear, step-by-step answers

Tone: [Friendly/Professional/Casual]
Response: Keep it concise and actionable

If you can't solve the issue, politely inform the customer 
to contact [support@yourcompany.com] for further assistance.
```

### Step 9: Test End-to-End
1. Send a test email to your support inbox
2. Wait for trigger interval
3. Check n8n executions
4. Verify response received
5. Check Google Sheet updated

### Step 10: Activate & Monitor
1. Activate the workflow
2. Monitor first 10-20 emails closely
3. Adjust system prompt as needed
4. Refine knowledge base content
5. Track costs daily

---

## ⚙️ Configuration

### Email Settings
```yaml
Trigger Interval: 5 minutes (adjust based on volume)
Max Emails per Execution: 10
Mark as Read: Yes
Move to Folder: "Processed" (optional)
```

### AI Agent Settings
```yaml
Model: gpt-4o-mini
Max Tokens: 500 (keep responses concise)
Temperature: 0.7 (balance creativity/consistency)
System Message: [See setup guide]
Timeout: 30 seconds
```

### Knowledge Base Settings
```yaml
Collection: starter_kb
Vector Size: 1536 (for text-embedding-3-small)
Distance Metric: Cosine
Top K Results: 3
Min Score: 0.7 (relevance threshold)
```

### Response Settings
```yaml
From Name: "Support Team"
Reply-To: support@yourcompany.com
Signature: Auto-append company signature
Format: HTML (for better formatting)
```

### Cost Limits (Optional)
Add a node to check daily costs:
```javascript
// Check if daily cost exceeds limit
if ($json.daily_cost > 10) {
  // Send alert email
  // Pause workflow
  // Notify admin
}
```

---

## 📊 Usage

### Daily Operations

#### Morning Check
1. Review Google Sheet metrics
2. Check any failed executions
3. Monitor response quality (sample 2-3 emails)
4. Note any recurring questions (add to KB)

#### Weekly Review
1. Analyze cost trends
2. Measure automation rate
3. Update knowledge base
4. Refine system prompt based on feedback

#### Monthly Optimization
1. Review total costs vs budget
2. Compare to previous month
3. Identify improvement areas
4. Plan knowledge base expansion

### Monitoring Metrics

Track these KPIs in your Google Sheet:
- **Volume**: Emails processed per day
- **Cost**: Total daily AI costs
- **Tokens**: Average tokens per interaction
- **Response Time**: Time from email to response

### Adding New Knowledge

When you notice recurring questions:
1. Create a new document with the answer
2. Upload to your knowledge base workflow
3. Re-run the embedding process
4. Test with a similar query
5. Deploy updated KB

### Handling Failures

Common failure scenarios:
- **Email not triggering**: Check inbox connection
- **AI timeout**: Reduce max tokens or complexity
- **KB not returning results**: Check relevance threshold
- **Cost spike**: Review unusual queries

---

## 💰 Cost Analysis

### Breakdown per Email

**GPT-4o-mini**:
- Input: 200 tokens × $0.00015/1K = $0.00003
- Output: 300 tokens × $0.0006/1K = $0.00018
- **Total**: ~$0.00021 per email

**Embeddings** (query):
- 50 tokens × $0.00002/1K = $0.000001
- **Total**: ~$0.000001 per email

**Combined**: ~$0.00021 per email (₹0.018)

### Monthly Projections

| Volume/Day | Daily Cost | Monthly Cost |
|------------|-----------|--------------|
| 50 emails  | $1.05     | $31.50       |
| 100 emails | $2.10     | $63.00       |
| 200 emails | $4.20     | $126.00      |
| 500 emails | $10.50    | $315.00      |

### vs Traditional Support

**Starter Package (100 emails/day)**:
- Cost: ~$63/month
- Automation: 50-60%
- Response time: <60s

**1 Part-time Agent**:
- Cost: ~$1,500/month
- Handles: ~100 emails/day
- Response time: 2-4 hours

**Savings**: ~$1,437/month (96% reduction)

### Cost Optimization Tips

1. **Reduce token usage**:
   - Keep system prompt concise
   - Limit max output tokens to 300-500
   - Use shorter KB documents

2. **Optimize KB searches**:
   - Top K = 3 (not 5 or 10)
   - Higher relevance threshold (0.7+)
   - Better document chunking

3. **Batch processing**:
   - Process multiple emails at once
   - Reduce API calls overhead

4. **Monitor usage**:
   - Set daily budget alerts
   - Track unusual cost spikes
   - Review expensive queries

---

## 🔧 Troubleshooting

### Email Not Triggering

**Problem**: Workflow not detecting new emails

**Solutions**:
1. Check email credentials (password changed?)
2. Verify IMAP is enabled on email account
3. Check interval setting (too long?)
4. Test connection manually
5. Check inbox filters/rules

### AI Responses Too Long

**Problem**: Responses exceeding 500 tokens

**Solutions**:
1. Reduce `max_tokens` to 300
2. Update system prompt: "Keep responses under 3 paragraphs"
3. Add to prompt: "Be concise and direct"
4. Review KB content (too verbose?)

### Poor Answer Quality

**Problem**: AI giving incorrect or irrelevant answers

**Solutions**:
1. **Check knowledge base**:
   - Are relevant docs included?
   - Are docs up to date?
   - Is chunking effective?

2. **Improve system prompt**:
   - Add more specific instructions
   - Include examples of good responses
   - Define tone and style clearly

3. **Review query types**:
   - Are questions outside KB scope?
   - Need to add more documents?

### High Costs

**Problem**: Costs exceeding budget

**Solutions**:
1. Check for unusually long inputs/outputs
2. Review token usage per email
3. Optimize system prompt length
4. Reduce KB search results (Top K)
5. Consider stricter email filters

### KB Not Finding Results

**Problem**: Knowledge base returning no results

**Solutions**:
1. Check relevance threshold (try 0.5-0.6)
2. Verify KB collection has documents
3. Test embeddings manually
4. Review document content (too technical?)
5. Rephrase query in system prompt

---

## ❓ FAQ

### General

**Q: How long does setup take?**
A: 15-30 minutes for basic setup, 1-2 hours including KB preparation.

**Q: Can I use this with Gmail?**
A: Yes! Enable IMAP and use app-specific password.

**Q: What happens if AI can't answer?**
A: It will provide a generic response asking customer to contact support@yourcompany.com.

**Q: Is this GDPR compliant?**
A: You need to ensure your email and OpenAI usage complies. Review OpenAI's data policy.

### Technical

**Q: Can I use a different AI model?**
A: Yes, but GPT-4o-mini is optimized for cost. GPT-3.5-turbo is cheaper but lower quality.

**Q: How many emails can this handle?**
A: Tested up to 200-300/day. Beyond that, consider Pro package.

**Q: Can I add human review?**
A: Not in Starter. Upgrade to Pro for human-in-the-loop features.

**Q: What if my KB is very large (>10,000 docs)?**
A: Consider Pro/Enterprise with better vector database capacity.

### Costs

**Q: Are there hidden costs?**
A: Only OpenAI API costs. Qdrant free tier is sufficient. Google Sheets is free.

**Q: How do I track costs?**
A: Check OpenAI dashboard daily. Worksheet logs estimated costs.

**Q: What if I exceed budget?**
A: Add a cost limiter node to pause workflow when threshold reached.

### Upgrading

**Q: When should I upgrade to Pro?**
A: When you need:
- Multiple channels (chat, WhatsApp)
- Human review capabilities
- Higher volume (>200/day)
- Better AI (GPT-4o)
- Advanced analytics

**Q: Is upgrade easy?**
A: Yes, you can import Pro workflow and migrate your KB collection.

**Q: Can I run both packages?**
A: Yes, use different collections and email addresses.

---

## 📞 Support

### Resources
- 📚 [n8n Documentation](https://docs.n8n.io/)
- 🤖 [OpenAI API Docs](https://platform.openai.com/docs)
- 🗄️ [Qdrant Docs](https://qdrant.tech/documentation/)

### Getting Help
- 💬 [n8n Community](https://community.n8n.io/)
- 📧 Email: support@yourcompany.com
- 🎫 GitHub Issues (if open source)

### Feedback
We'd love to hear from you:
- ⭐ What's working well?
- 🐛 What issues did you face?
- 💡 What features do you need?
- 📈 Share your success metrics!

---

## 🎯 Next Steps

After successful setup:

1. **Week 1**: Monitor closely, refine prompts
2. **Week 2**: Expand knowledge base
3. **Week 3**: Analyze metrics, optimize costs
4. **Week 4**: Evaluate if ready for Pro upgrade

---

## 📄 License

[Your License Here]

---

**Version**: 1.0.0  
**Last Updated**: January 2026  
**Package**: Starter 💚