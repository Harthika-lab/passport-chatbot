# Passport Seva Chatbot - Backend (Python + Flask)

from flask import Flask, request, jsonify
import openai  # Using OpenAI API for chatbot intelligence

app = Flask(__name__)

# Configure OpenAI API (Replace 'YOUR_API_KEY' with actual API key)
openai.api_key = "YOUR_API_KEY"

def get_chatbot_response(user_query):
    response = openai.ChatCompletion.create(
        model="gpt-3.5-turbo",
        messages=[{"role": "user", "content": user_query}]
    )
    return response["choices"][0]["message"]["content"]

@app.route("/chat", methods=["POST"])
def chatbot():
    data = request.get_json()
    user_query = data.get("query")
    response = get_chatbot_response(user_query)
    return jsonify({"response": response})

if __name__ == "__main__":
    app.run(debug=True)

# Passport Seva Chatbot - Frontend (Java Spring Boot)

import org.springframework.web.bind.annotation.*;
import org.springframework.web.client.RestTemplate;
import org.springframework.http.ResponseEntity;

@RestController
@RequestMapping("/api/chat")
public class ChatbotController {

    @PostMapping
    public ResponseEntity<String> chatWithBot(@RequestBody String userQuery) {
        String flaskBackendUrl = "http://localhost:5000/chat";
        RestTemplate restTemplate = new RestTemplate();
        ResponseEntity<String> response = restTemplate.postForEntity(flaskBackendUrl, userQuery, String.class);
        return ResponseEntity.ok(response.getBody());
    }
}
