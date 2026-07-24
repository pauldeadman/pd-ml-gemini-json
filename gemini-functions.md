# Multi-Function Calling with Gemini 3 Flash

Note that the syntax changes between Gemini versions.

## First Request

Setup GEMINI_API_KEY in environment. Call first request

```
curl -X POST "https://generativelanguage.googleapis.com/v1beta/models/gemini-3-flash-preview:generateContent" \
-H "Content-Type: application/json" \
-H "x-goog-api-key: $GEMINI_API_KEY" \
-d '{
  "contents": [{
    "role": "user",
    "parts": [{
      "text": "What is the northernmost city in the United States? What'\''s the weather like there today?"
    }]
  }],
  "tools": [{
    "googleSearch": {}
  }, {
    "functionDeclarations": [{
      "name": "getWeather",
      "description": "Get the weather in a given location",
      "parameters": {
          "type": "OBJECT",
          "properties": {
              "location": {
                  "type": "STRING",
                  "description": "The city and state, e.g. San Francisco, CA"
              }
          },
          "required": ["location"]
      }
    }]
  }],
  "toolConfig": {
    "includeServerSideToolInvocations": true
  }
}'
```
## First Output

The output includes multiple components

![Result1](/Images/Result1.png)

## Second Request

Copy PARTS into new request and then submit that.

![SendResult](/Images/Gemini-3-function-send2.png)

## Final Result

The northernmost city in the United States is **Utqiaġvik, Alaska** (formerly known as Barrow).\n\nAs of today, the weather there is **very cold**, with a current temperature of approximately **22°F (-6°C)**. \n\nLocated about 320 miles north of the Arctic Circle, Utqiaġvik is one of the northernmost cities in the world and experiences extreme seasonal shifts, including 65 days of complete darkness in the winter and 80 days of continuous sunlight in the summer.

```
      "content": {
        "parts": [
          {
            "text": "The northernmost city in the United States is **Utqiaġvik, Alaska** (formerly known as Barrow).\n\nAs of today, the weather there is **very cold**, with a current temperature of approximately **22°F (-6°C)**. \n\nLocated about 320 miles north of the Arctic Circle, Utqiaġvik is one of the northernmost cities in the world and experiences extreme seasonal shifts, including 65 days of complete darkness in the winter and 80 days of continuous sunlight in the summer.",
            "thoughtSignature": "Ers.......nesxAwLl1tV6z"
          }
        ],
```

Note: Thought Signature has been redacted.

![FinalResult](/Images/Gemini-3-function-result2.png)
