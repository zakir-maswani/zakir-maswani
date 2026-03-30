# Rules-Based Chatbot with Human-in-the-Loop (HITL)

A demonstration of the Human-in-the-Loop concept using LangChain and Streamlit. This chatbot shows how AI systems can be augmented with human oversight for sensitive operations.

## 🎯 Concept Overview

The **Human-in-the-Loop (HITL)** pattern integrates human judgment into AI workflows:

1. **Automatic Handling**: Simple inquiries are processed automatically by the bot
2. **Human Review Queue**: Sensitive actions (refunds, account changes, escalations) are flagged for human review
3. **Human Decision**: A human operator can approve, reject, or modify the bot's proposed action
4. **Feedback Loop**: Human decisions help improve the system over time

This approach balances automation efficiency with human oversight for critical decisions.

## 🏗️ Architecture

```
User Input
    ↓
Intent Detection (Rule-based)
    ↓
LangChain Response Generation
    ↓
Action Type Classification
    ├─ Simple Response → Direct Response
    └─ Sensitive Action → Pending Review Queue
         ↓
    Human Review Panel
         ↓
    Approve/Reject/Modify
         ↓
    Action History
```

## 🚀 Getting Started

### Prerequisites

- Python 3.8+
- OpenAI API key (set as `OPENAI_API_KEY` environment variable)

### Installation

1. **Clone or download this project**

2. **Install dependencies**:
   ```bash
   pip install streamlit langchain langchain-openai
   ```

3. **Set your OpenAI API key**:
   ```bash
   export OPENAI_API_KEY="your-api-key-here"
   ```

### Running the Application

```bash
streamlit run hitl_chatbot.py
```

The app will open in your browser at `http://localhost:8501`

## 💬 Usage Guide

### Chat Interface (Left Panel)

1. Type your message in the chat input box
2. The bot will process your message and generate a response
3. If the message triggers a sensitive action, it will be flagged for review

### Human Review Panel (Right Panel)

#### Pending Review Section
- Shows all actions awaiting human decision
- Each action displays:
  - **Action ID**: Unique identifier
  - **Type**: Category of action (refund, account_change, escalation, password_reset)
  - **Intent**: Detected user intent
  - **User Message**: Original customer message
  - **Bot Response**: AI-generated response
  - **Timestamp**: When the action was created

- **Approve Button**: Accept the bot's proposed action
  - Optionally add approval notes
  - Action moves to "Approved Actions" history

- **Reject Button**: Decline the bot's proposed action
  - Add reason for rejection
  - Action moves to "Rejected Actions" history

#### Action History Section
- **Approved Actions Tab**: All approved actions with human notes
- **Rejected Actions Tab**: All rejected actions with rejection reasons

#### Statistics
- Total messages exchanged
- Pending actions count
- Approved actions count
- Rejected actions count

## 🎯 Example Interactions

### Example 1: Simple Inquiry (No Review Needed)
```
User: "What are your business hours?"
Bot: "We're open Monday-Friday, 9 AM to 6 PM EST."
Status: ✅ Handled automatically
```

### Example 2: Refund Request (Requires Review)
```
User: "I'd like a refund for my order"
Bot: "I understand you'd like a refund. I can help process that."
Status: ⏳ Pending human review
Human Action: Approve with notes "Customer has valid receipt"
```

### Example 3: Account Change (Requires Review)
```
User: "Can you change my email address?"
Bot: "I can help you update your email address."
Status: ⏳ Pending human review
Human Action: Reject with reason "Requires identity verification first"
```

## 🔧 Customization

### Adding New Intents

Edit the `detect_intent_and_action()` function:

```python
def detect_intent_and_action(user_message: str) -> tuple[str, ActionType]:
    message_lower = user_message.lower()
    
    # Add new intent detection
    if any(word in message_lower for word in ["your_keywords"]):
        return "your_intent", ActionType.YOUR_ACTION_TYPE
    
    return "general_inquiry", ActionType.SIMPLE_RESPONSE
```

### Adding New Action Types

1. Add to the `ActionType` enum:
```python
class ActionType(Enum):
    YOUR_ACTION = "your_action"
```

2. Update intent detection to use the new type

### Customizing the System Prompt

Modify the `generate_bot_response()` function's `system_prompt` to change bot behavior:

```python
system_prompt = """Your custom system prompt here.
Current intent: {intent}"""
```

## 📊 Key Features

✅ **Rule-Based Intent Detection**: Simple, interpretable keyword-based classification
✅ **LangChain Integration**: Leverages OpenAI for natural language generation
✅ **Human Review Queue**: Clear interface for pending action review
✅ **Action Tracking**: Complete history of approved and rejected actions
✅ **Real-Time Statistics**: Monitor chatbot activity and human decisions
✅ **Streamlit UI**: Interactive, responsive web interface
✅ **Session State Management**: Persistent conversation history

## 🔐 Security Considerations

- **API Keys**: Never commit your OpenAI API key. Use environment variables.
- **Data Privacy**: This demo stores conversations in session state (not persistent). For production, use a database.
- **Human Oversight**: All sensitive actions require explicit human approval before execution.
- **Audit Trail**: All actions are tracked with timestamps and human notes.

## 🚀 Production Considerations

For a production deployment, consider:

1. **Database Integration**: Store conversations and actions in a persistent database
2. **Authentication**: Add user authentication and role-based access control
3. **Logging**: Implement comprehensive logging for audit trails
4. **Rate Limiting**: Add rate limiting to prevent abuse
5. **Error Handling**: Implement robust error handling and fallback mechanisms
6. **Monitoring**: Add monitoring and alerting for system health
7. **Scalability**: Use a production-grade web framework (FastAPI, Django)
8. **Cost Control**: Implement token counting and cost monitoring for LLM calls

## 📚 References

- [LangChain Documentation](https://python.langchain.com/)
- [Streamlit Documentation](https://docs.streamlit.io/)
- [OpenAI API Reference](https://platform.openai.com/docs/api-reference)
- [Human-in-the-Loop ML](https://en.wikipedia.org/wiki/Human-in-the-loop)

## 📝 License

This project is provided as-is for educational purposes.

## 🤝 Contributing

Feel free to extend this project with:
- More sophisticated NLP for intent detection
- Database integration for persistence
- Multi-user support with role-based access
- Advanced action workflows
- Analytics and reporting

---

**Happy chatting! 🚀**
