# 🤖 n8n — Private AI Email Assistant

একটি প্রাইভেট AI Email Assistant তৈরির ধাপে ধাপে গাইড।  
এই ডকুমেন্টটি ভবিষ্যতে রেফারেন্স হিসেবে ব্যবহার করার জন্য তৈরি।

---

## 📋 ওভারভিউ

| বিষয় | বিবরণ |
|---|---|
| **টুল** | n8n Workflow Automation |
| **উদ্দেশ্য** | Gmail থেকে Email পড়ে AI দিয়ে প্রসেস করা |
| **AI Model** | Google Gemini (via n8n) |
| **ট্রিগার** | নতুন Email আসলে স্বয়ংক্রিয়ভাবে চালু হবে |

---

## 🔧 ধাপ ১ — টেস্ট ইমেইল পাঠানো

> **উদ্দেশ্য:** Workflow টেস্ট করার জন্য একটি নমুনা Email পাঠানো।

**কাজ:**
- `mdwasimu02@gmail.com` থেকে `mdwasimu015@gmail.com`-এ একটি টেস্ট Email পাঠাও।

---

## ⚙️ ধাপ ২ — Gmail Trigger সেটআপ

> **উদ্দেশ্য:** নতুন Email আসলে Workflow স্বয়ংক্রিয়ভাবে শুরু হবে।

**ধাপসমূহ:**

1. **Add first step** বাটনে ক্লিক করো।
2. Search বক্সে টাইপ করো: `gmail`
3. **On message received** ক্লিক করো।
4. **Simplify** অপশনটি **OFF** করো।
5. **Fetch Test Event** বাটনে ক্লিক করো।

```
📌 নোট: Simplify OFF না করলে Email-এর পুরো raw data পাবে না।
```

---

## ⚙️ ধাপ ৩ — HTML Content Extract করা

> **উদ্দেশ্য:** Email-এর Body থেকে plain text বের করা।

**ধাপসমূহ:**

1. Gmail Trigger নোডের **`+`** চিহ্নে ক্লিক করো।
2. Search বক্সে টাইপ করো: `HTML`
3. **Extract HTML Content** ক্লিক করো।
4. নিচের মতো ফিল্ডগুলো পূরণ করো:

| ফিল্ড | মান |
|---|---|
| **Key** | `email_body` |
| **CSS Selector** | `body` |
| **JSON Property** | Email-এর `text` field টি টেনে এনে বসাও |

5. **Execute Step** বাটনে ক্লিক করো।

```
📌 নোট: JSON Property-তে পাশের প্যানেল থেকে text field টি drag করে বসাতে হবে।
```

---

## ⚙️ ধাপ ৪ — AI Agent সেটআপ

> **উদ্দেশ্য:** Email-এর content AI Agent-এর কাছে পাঠানো এবং স্মার্ট রিপ্লাই বা বিশ্লেষণ পাওয়া।

**ধাপসমূহ:**

1. HTML নোডের **`+`** চিহ্নে ক্লিক করো।
2. Search বক্সে টাইপ করো: `AI Agent`
3. **AI Agent** নোডে ক্লিক করো।
4. নিচের মতো কনফিগার করো:

| ফিল্ড | মান |
|---|---|
| **Source for Prompt (User Message)** | `Define below` সিলেক্ট করো |
| **Prompt (User Message)** | `prompt.txt` থেকে টেক্সট কপি করে পেস্ট করো |

5. Prompt টেক্সট থেকে `{{ $json.email_body }}` অংশটি **মুছে দাও**।
6. পাশের প্যানেল থেকে **email_body** field টি টেনে এনে সেই জায়গায় বসাও।

```
📌 নোট: এটি নিশ্চিত করবে যে HTML নোড থেকে extract করা email body-টি
         AI Agent-এর prompt-এ dynamically যুক্ত হচ্ছে।
```

---

## ⚙️ ধাপ ৫ — Google Gemini Chat Model সংযোগ

> **উদ্দেশ্য:** AI Agent-কে Google Gemini দিয়ে চালানো।

**ধাপসমূহ:**

1. AI Agent নোডের **Chat Model** সেকশনের **`+`** চিহ্নে ক্লিক করো।
2. Search বক্সে টাইপ করো: `Google Gemini Chat Model`
3. **Google Gemini Chat Model** ক্লিক করো।
4. নিচের সেটআপ করো:

| ফিল্ড | কাজ |
|---|---|
| **Credential** | Google Gemini API Key সেট করো |
| **Model** | পছন্দমতো Gemini Model সিলেক্ট করো (যেমন: `gemini-1.5-flash`) |

```
📌 নোট: Credential সেট করতে Google AI Studio থেকে API Key নিতে হবে।
         লিংক: https://aistudio.google.com/app/apikey
```

---

## ⚙️ ধাপ ৬ — AI-Generated Reply Email পাঠানো

> **উদ্দেশ্য:** AI Agent-এর তৈরি করা রিপ্লাই Gmail-এর মাধ্যমে Sender-কে ফেরত পাঠানো।

**ধাপসমূহ:**

1. AI Agent নোডের **`+`** চিহ্নে ক্লিক করো।
2. Search বক্সে টাইপ করো: `gmail`
3. **Send a message** ক্লিক করো।
4. নিচের মতো ফিল্ডগুলো পূরণ করো:

| ফিল্ড | মান |
|---|---|
| **To** | পাশের প্যানেল থেকে Gmail Trigger-এর **sender-এর email** field টি টেনে এনে বসাও |
| **Subject** | পাশের প্যানেল থেকে Gmail Trigger-এর **subject** field টি টেনে এনে বসাও |
| **Email Type** | `Text` সিলেক্ট করো |
| **Message** | `{{ $json.output }}` টাইপ করো |

```
📌 নোট: {{ $json.output }} হলো AI Agent-এর generate করা রিপ্লাই।
         এটি সরাসরি টাইপ করতে হবে অথবা পাশের প্যানেল থেকে output field টেনে আনো।
```

---

## 🔗 সম্পূর্ণ Workflow ফ্লো

```
Gmail Trigger
  (নতুন Email এলে)
       ↓
Extract HTML Content
  (email_body বের করা)
       ↓
AI Agent
  (prompt + email_body দিয়ে AI-কে জিজ্ঞেস করা)
       ↓
Google Gemini Chat Model
  (AI রিপ্লাই তৈরি করবে)
       ↓
Gmail — Send a message
  (AI-এর রিপ্লাই Sender-কে পাঠানো)
```

---

## ✅ চেকলিস্ট

- [ ] টেস্ট Email পাঠানো হয়েছে
- [ ] Gmail Trigger সেটআপ হয়েছে (Simplify OFF)
- [ ] HTML Extract নোড কনফিগার হয়েছে
- [ ] AI Agent-এ prompt সেট হয়েছে
- [ ] email_body dynamically যুক্ত হয়েছে
- [ ] Google Gemini Credential সেট হয়েছে
- [ ] Gmail Send নোড কনফিগার হয়েছে (To, Subject, Message)
- [ ] সম্পূর্ণ Workflow টেস্ট করা হয়েছে

---

## 🛠️ সম্ভাব্য সমস্যা ও সমাধান

| সমস্যা | সমাধান |
|---|---|
| Gmail Trigger কাজ করছে না | Gmail Account-এ OAuth Permission দিয়েছ কিনা চেক করো |
| email_body খালি আসছে | CSS Selector `body` ঠিক আছে কিনা দেখো |
| Gemini API Error | API Key সঠিকভাবে দেওয়া আছে কিনা যাচাই করো |
| AI Agent থেকে Response আসছে না | Prompt সঠিকভাবে কনফিগার হয়েছে কিনা দেখো |

---
