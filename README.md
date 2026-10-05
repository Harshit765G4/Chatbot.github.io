# 🤖 TechMaa AI Customer Support Chatbot

An AI-powered customer support interface embedded into the TechMaa-AI website. The project combines a responsive corporate landing page with a floating chatbot that uses the Google Gemini API to generate conversational support responses.

## ✨ Overview

This project is implemented as a self-contained static web page. The main experience lives in `index.html`, which contains the TechMaa-AI homepage, styling, UI behavior, and chatbot logic.

The chatbot is designed to:

- Open from a floating chat button.
- Provide a responsive chat popup for desktop and mobile layouts.
- Maintain conversation history during the current browser session.
- Show typing feedback while waiting for an AI response.
- Provide suggested questions when a conversation starts.
- Support a Clear Chat action that resets the conversation.
- Send prompts to Google's Gemini Generative Language API.
- Format basic Markdown-style responses such as bold text and lists.
- Present timestamps and separate visual styles for user and assistant messages.

## 🚀 Key Features

### AI Customer Support
The chatbot uses a TechMaa-specific system prompt that defines a professional customer-support persona for **TechMaa AI Innovation Pvt Ltd**. The prompt instructs the assistant to understand intent, maintain context, respond empathetically, and represent the company positively.

### Conversation Context
Messages are stored in a JavaScript `chatHistory` array and sent with subsequent requests, allowing the assistant to use earlier messages from the current conversation.

### Interactive Chat UI
The interface includes:

- Floating chat launcher
- Expandable chat window
- Close button
- Message input and send button
- Typing indicator
- Suggested questions
- Clear/reset conversation
- Responsive message bubbles
- Timestamps
- Disabled input controls while a request is being processed

### TechMaa-AI Website
The page surrounding the chatbot provides a corporate website experience with sections for services, industries, outcomes, testimonials, and company navigation.

The page also links to other site pages such as:

- About Us
- Services
- Blog
- Testimonials
- Insights
- Careers
- Team
- Contact

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Markup | HTML5 |
| Client logic | Vanilla JavaScript (ES6+) |
| Styling | Tailwind CSS via CDN + custom CSS |
| AI | Google Gemini Generative Language API |
| Icons | Font Awesome |
| Animations | AOS (Animate On Scroll) |
| UI components | Swiper |
| Fonts | Google Fonts / Inter |
| Hosting model | Static website / GitHub Pages compatible |

No Node.js server, package manager, build process, or separate backend is required by the current implementation.

## 📁 Project Structure

```text
Chatbot.github.io/
├── index.html
├── README.md
└── TechMaa AI Customer Support Chatbot.pdf
```

### Main Files

**`index.html`**
Contains the complete application, including:

- TechMaa-AI homepage markup
- Responsive navigation
- Custom styling
- Chatbot markup
- Chatbot state management
- Gemini API request handling
- Suggested-question generation
- Chat reset behavior
- Typing indicator
- Basic response formatting

**`TechMaa AI Customer Support Chatbot.pdf`**
Project documentation/reference material included in the repository.

## 🔄 Chatbot Flow

The runtime flow is approximately:

1. The page loads and initializes an empty conversation.
2. A welcome message is displayed.
3. Suggested customer questions are presented.
4. The visitor submits a message or selects a suggestion.
5. The message is added to `chatHistory`.
6. A typing indicator is displayed and the input is temporarily disabled.
7. The browser sends the conversation plus the system instruction to Gemini.
8. The returned text is added to the conversation history.
9. The assistant message is rendered in the chat window.
10. The controls are re-enabled for the next message.

## 🔌 Gemini API Integration

The current implementation calls the Gemini REST API directly from browser-side JavaScript.

Conceptually, the request contains:

```javascript
{
  contents: chatHistory,
  systemInstruction: {
    parts: [{ text: systemPrompt }]
  },
  generationConfig: {
    temperature: 0.7,
    maxOutputTokens: 2048
  }
}
```

The current code targets a Gemini Flash model endpoint and passes the API key as a query parameter.

## ▶️ Run Locally

Because the project is static, no dependency installation is required.

### Option 1 — Open directly

Open `index.html` in a modern web browser.

### Option 2 — Use VS Code Live Server

For development, open the repository in VS Code and launch `index.html` with the **Live Server** extension.

A local server is generally preferable during development because it behaves more like a deployed website and makes browser debugging easier.

## 🌐 Deployment

The repository structure is compatible with static hosting platforms such as:

- GitHub Pages
- Netlify
- Vercel static hosting
- Cloudflare Pages
- Any web server capable of serving static HTML

Before deployment, verify the Gemini integration, external CDN availability, and browser CORS/network behavior.

## ⚠️ Security Notice

**Do not expose a production Gemini API key in client-side JavaScript.**

The current `index.html` contains the Gemini API key directly in browser code. Any visitor can inspect the page source or browser network activity and potentially retrieve the key.

For a production deployment, move the Gemini request behind a server-side API or serverless function:

```text
Browser
   │
   │ HTTPS request
   ▼
Your Backend / Serverless Function
   │
   │ Gemini API request
   ▼
Google Gemini API
```

The server should keep the API key in an environment variable and return only the required response to the browser.

Recommended production improvements include:

- Revoke or rotate the currently exposed API key.
- Store secrets in environment variables.
- Add authentication/rate limiting where appropriate.
- Apply request quotas and abuse protection.
- Validate and limit user input.
- Keep provider credentials out of Git history and frontend bundles.
- Consider server-side logging with privacy controls.

## 🛡️ Client-Side Safety

The current UI escapes `<` and `>` characters before inserting messages into the chat window, which helps reduce direct HTML injection when rendering user/assistant text.

The bot then applies a small amount of formatting for bold text and list-like responses. This should still be treated as lightweight client-side formatting rather than a full Markdown sanitizer.

## ⚙️ Customization

The chatbot can be customized directly in `index.html`.

### System prompt

Update the `systemPrompt` constant to change:

- Assistant personality
- Company information
- Support behavior
- Domain knowledge
- Response style

### Suggested questions

Edit the suggestions array used during chat initialization to change the quick-start prompts.

### Visual design

The chat layout and animations can be customized through the page's Tailwind utility classes and embedded CSS.

## 🧪 Error Handling

When the Gemini request fails, the chatbot:

- Logs the error to the browser console.
- Removes the typing indicator.
- Displays a fallback connection message.
- Re-enables the input controls.

This provides a graceful client-side failure path, but production applications should also implement server-side monitoring, retries where appropriate, and structured error reporting.

## 📌 Current Limitations

The current repository is intentionally lightweight, but there are several production limitations:

1. **API key exposure:** Gemini credentials are embedded in frontend JavaScript.
2. **No backend proxy:** API calls are made directly from the browser.
3. **No persistent storage:** Conversation history is kept only in in-memory JavaScript state and is lost when the chat is reset or the page is reloaded.
4. **No authentication:** The chatbot is publicly accessible.
5. **No server-side rate limiting:** Abuse controls are not implemented in the repository.
6. **Single-file architecture:** Website markup, styles, and application logic are concentrated in `index.html`.
7. **External CDN dependency:** Tailwind, fonts, icons, AOS, and Swiper are loaded from external services.

## 🔮 Future Improvements

A stronger production version could introduce:

- Secure Gemini integration through a backend/API route
- Environment-based configuration
- Streaming assistant responses
- Conversation persistence
- Customer/session IDs
- Backend analytics and conversation metrics
- Authentication and role-based support tooling
- Rate limiting and abuse detection
- Knowledge-base or RAG integration
- Retrieval from TechMaa-AI services, FAQs, policies, and documentation
- Better Markdown rendering with a trusted sanitizer
- Automated testing
- Modular JavaScript files instead of a single-page script
- Accessibility improvements and keyboard-first navigation
- Observability, logging, and alerting

## 📚 Learning Goals

This repository demonstrates practical concepts in:

- Frontend web development
- DOM manipulation
- JavaScript asynchronous programming
- REST API integration
- Conversational AI integration
- Client-side state management
- Responsive UI design
- Error handling
- Basic input/output sanitization

## 📄 License

No `LICENSE` file is currently present in the repository, so a formal open-source license should not be assumed. Add a license file and update this section when the licensing terms are decided.

## 👨‍💻 Author

**Harshit Garg**

GitHub: [@Harshit765G4](https://github.com/Harshit765G4)

---

⭐ If this project helps you learn or experiment with AI-powered customer support interfaces, consider starring the repository.