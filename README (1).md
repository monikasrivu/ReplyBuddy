
# 📄 Project Documentation: ReplyBuddy – WhatsApp Auto-Responder Using n8n

## 🧠 Overview
**ReplyBuddy** is an automated WhatsApp responder built using **n8n Cloud** and **WhatsApp Cloud API**. It’s designed to automatically respond to incoming messages (like "hi") and initiate a **personalized 5-day follow-up message cycle**. It’s ideal for businesses, organizations, and services aiming to onboard or inform customers through scheduled WhatsApp messages.

## 🎯 Project Goals
1. **Auto-reply to “hi” messages** with a welcome message.
2. **Start a 5-day message cycle** after the first "hi".
3. **Log user data** only once in Google Sheets.
4. **Stop the cycle** immediately if the user sends "stop".
5. Ensure that "hi" does **not restart the cycle** after "stop".
6. Designed for scaling to real-time production use.

## 🛠️ Technologies Used
| Tool | Purpose |
|------|---------|
| **n8n Cloud** | Workflow automation |
| **WhatsApp Cloud API (Meta)** | Messaging platform |
| **Google Sheets** | User data storage |
| **JavaScript (n8n Function nodes)** | Logic & control |

## 📁 Files/Workflows Used
| File | Purpose |
|------|--------|
| `My workflow 12 (4).json` | Main auto-reply and logging workflow |
| `ReplyBuddy - Daily Message Cycle.json` | Scheduled daily follow-up messages |

## 📐 Google Sheet Structure
**Sheet Name:** `Sheet1`  
**Columns:**
| Column         | Description |
|----------------|-------------|
| `user_id`      | WhatsApp sender's ID (phone number) |
| `start_date`   | Date of initial “hi” |
| `stopped`      | `true` or `false` – marks if user ended the cycle |
| `last_sent_day`| The last day message sent (0–5) |
| `row_number`   | Used internally for update operations |

## 🔁 Workflow 1: Auto-Reply & Logger 
**Trigger:** Webhook when a message is received on WhatsApp.

**Key Steps:**
1. **Code Node** – Detects `hi` or `stop`.
2. **Switch Node** – Routes based on message type.
3. **hi Flow** – Checks if user exists → adds if not → sends reply.
4. **stop Flow** – Retrieves row → updates `stopped = true`.

## 🔁 Workflow 2: Daily Message Cycle 
**Trigger:** Scheduled (once daily).

**Key Steps:**
1. Reads all users from Google Sheet.
2. Filters: `stopped != "true"` and `last_sent_day < 5`.
3. Generates daily message based on `last_sent_day`.
4. Sends via WhatsApp API.
5. Updates `last_sent_day`.

## ✅ Important Behaviors
| Action | Expected Result |
|--------|------------------|
| User sends `hi` | Sends reply + starts cycle |
| User sends `stop` | Stops cycle immediately |
| `hi` after stop | No reply |
| Multiple users | Independent cycles |
| Sheet logs | Only one entry per user |

## 📦 Deployment Notes
- Uses Meta’s Test Number now.
- For live: verify business, add real number, set tokens.

## 📋 Suggested Enhancements
| Task | Benefit |
|------|--------|
| Real business number | Send to anyone |
| Analytics | Track usage |
| Error handling | Reliability |
| Dashboard | Easy monitoring |

## 🔐 Security Note
- Keep API tokens and Sheet URL safe.
