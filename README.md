prime_ai/
│
├── lib/
│   ├── main.dart
│   ├── screens/
│   │   ├── login.dart
│   │   ├── signup.dart
│   │   ├── home.dart
│   │   ├── chat.dart
│   │   ├── image_generate.dart
│   │   ├── image_analyze.dart
│   │   └── subscription.dart
│   ├── services/
│   │   ├── auth_service.dart
│   │   ├── chat_service.dart
│   │   ├── image_service.dart
│   │   └── voice_service.dart
│   └── widgets/
│
├── backend/
│   ├── server.js
│   ├── routes/
│   └── controllers/
│
└── pubspec.yaml
flutter create prime_ai
import 'package:firebase_auth/firebase_auth.dart';

class AuthService {
  final FirebaseAuth _auth = FirebaseAuth.instance;

  Future<User?> login(String email, String password) async {
    final result = await _auth.signInWithEmailAndPassword(
      email: email,
      password: password, );
    return result.user;
  Future<User?> signup(String email, String password) async {
    final result = await _auth.createUserWithEmailAndPassword(
      email: email,
      password: password );
    return result.user }
import 'dart:convert';
import 'package:http/http.dart' as http;

class ChatService {
  Future<String> sendMessage(String message) async {
    final response = await http.post(
      Uri.parse('https://yourbackend.com/chat'),
      headers: {'Content-Type': 'application/json'},
      body: jsonEncode({'message': message}),
    );

    final data = jsonDecode(response.body);
    return data['reply'];
  }
}
Future<String> generateImage(String prompt) async {
  final response = await http.post(
    Uri.parse('https://yourbackend.com/generate-image'),
    body: jsonEncode({'prompt': prompt}),
  );

  return jsonDecode(response.body)['image_url'];
}

Future<String> analyzeImage(String imagePath) async {
  final response = await http.post(
    Uri.parse('https://yourbackend.com/analyze'),
    body: jsonEncode({'image': imagePath}),
  );

  return jsonDecode(response.body)['analysis'];
}
import 'package:speech_to_text/speech_to_text.dart';

Future<void> listenVoice() async {
  SpeechToText speech = SpeechToText();
  await speech.initialize();

  speech.listen(onResult: (result) {
    print(result.recognizedWords);
  });
}
const express = require('express');
const axios = require('axios');
const app = express();
app.use(express.json());

app.post('/chat', async (req, res) => {
  const response = await axios.post(
    'https://api.openai.com/v1/chat/completions',
    {
      model: 'gpt-4o-mini',
      messages: [{ role: 'user', content: req.body.message }],
    },
    {
      headers: { 'Authorization': 'Bearer YOUR_OPENAI_API_KEY' }
    }
  );

  res.json({
    reply: response.data.choices[0].message.content
  });
});

app.listen(3000);
import 'package:razorpay_flutter/razorpay_flutter.dart';

Razorpay razorpay = Razorpay();
razorpay.on(Razorpay.EVENT_PAYMENT_SUCCESS, (PaymentSuccessResponse response) {
  print('Payment Success: ${response.paymentId}');
});
razorpay.on(Razorpay.EVENT_PAYMENT_ERROR, (PaymentFailureResponse response) {
  print('Payment Error: ${response.code} - ${response.message}');
});
