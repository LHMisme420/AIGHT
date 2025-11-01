# AIGHT
AI safety for teens
safe-mind/
├── README.md
├── LICENSE
├── docs/
│   ├── curriculum.md
│   └── educator-guide.md
├── app/
│   ├── App.tsx
│   ├── package.json
│   ├── tsconfig.json
│   ├── app.config.js
│   ├── src/
│   │   ├── screens/
│   │   │   ├── HomeScreen.tsx
│   │   │   ├── ModuleScreen.tsx
│   │   │   ├── LessonScreen.tsx
│   │   │   ├── QuizScreen.tsx
│   │   │   ├── ProfileScreen.tsx
│   │   │   └── AdminScreen.tsx
│   │   ├── components/
│   │   │   ├── LessonCard.tsx
│   │   │   ├── Badge.tsx
│   │   │   ├── ProgressBar.tsx
│   │   │   └── Layout.tsx
│   │   ├── hooks/
│   │   │   └── useLesson.ts
│   │   ├── lib/
│   │   │   ├── api.ts
│   │   │   └── auth.ts
│   │   └── data/
│   │       └── lessons.json
├── backend/
│   ├── firebase.json
│   ├── firestore.rules
│   ├── functions/
│   │   ├── package.json
│   │   └── src/
│   │       └── issueCredential.ts
│   └── supabase/
│       └── schema.sql
├── onchain/
│   └── solana/
│       ├── anchor_root.ts
│       └── package.json
└── data/
    └── lessons.seed.json
# SAFE MIND – Mandatory AI Safety Learning for Teens

Safe Mind is a mobile-first learning app that teaches AI literacy, digital responsibility, bias awareness, and ethical citizenship to students 13–18.

This repo contains:

- 📱 `app/` – React Native / Expo front-end
- 🔐 `backend/` – Firebase/Supabase backend + credential function
- ⛓ `onchain/solana/` – script to anchor completion Merkle roots to Solana (memo program)
- 📚 `docs/` – full 6-module curriculum + educator guide
- 🗂 `data/` – starter JSON for lessons

## Features
- 6 core modules
- Gamified badges: Data Guardian, Bias Breaker, Ethical Coder
- Student progress tracking
- Optional blockchain verification (Solana)
- Educator dashboard view (MVP-level)

## Getting Started

### 1. Frontend
```bash
cd app
npm install
npm run start
# or
npx expo start
cd backend/functions
npm install
npm run build
firebase deploy --only functions
cd onchain/solana
npm install
npm run start

---

## 3. `LICENSE`

```text
MIT License

Copyright (c) 2025 ...

Permission is hereby granted, free of charge, to any person obtaining a copy
...
# SAFE MIND Curriculum

## Module 1 – What Is AI? (The Machine Mind)
**Goal:** Students understand how AI systems are trained and where they fail.

- Lesson 1.1: Data → Pattern → Prediction
- Lesson 1.2: AI vs Automation vs Human Intelligence
- Lesson 1.3: Limits of AI (hallucinations, overfitting)
- Activity: Train-a-model demo (images/text)
- Assessment: 6-question quiz + reflection “When *shouldn’t* we let AI decide?”

## Module 2 – Digital Responsibility (You Are the Data)
**Goal:** Students understand that data = power.

- Lesson 2.1: Data trails & metadata
- Lesson 2.2: Privacy and consent
- Lesson 2.3: Phishing & social engineering
- Activity: “24h Data Map”
- Assessment: Create a 5-rule Digital Safety Pledge

## Module 3 – Bias & Fairness (Teaching Machines to Be Just)
**Goal:** Students can recognize bias and suggest fixes.

- Lesson 3.1: Human → dataset → model bias
- Lesson 3.2: Case studies (facial recognition, hiring)
- Lesson 3.3: Explainability & transparency
- Activity: “Fix the unfair model”
- Assessment: short scenario-based quiz

## Module 4 – AI & Society (Media, Work, Democracy)
**Goal:** Students can spot deepfakes, misinformation, and automation risks.

- Lesson 4.1: Deepfakes & impersonation
- Lesson 4.2: Misinformation patterns
- Lesson 4.3: Future of work
- Activity: “Real or AI?” media challenge
- Assessment: 1-page media analysis

## Module 5 – Human Resilience (Mind over Machine)
**Goal:** Students learn to protect attention, empathy, and purpose.

- Lesson 5.1: The attention economy
- Lesson 5.2: Digital wellness
- Lesson 5.3: Faith / values / purpose
- Activity: 1-day Digital Sabbath
- Assessment: Reflection journal

## Module 6 – AI Citizenship (Law, Rights, Future We Build)
**Goal:** Students become ethical participants.

- Lesson 6.1: AI rights & regulations (age-appropriate)
- Lesson 6.2: Community tech & social good
- Lesson 6.3: Draft an AI Bill of Rights
- Activity: Group policy proposal
- Assessment: Final quiz + project

## Completion
- Student earns: **Safe Mind Certificate of AI Literacy**
- Backend emits: `completion_hash` → sent to Solana for anchoring
import React from "react";
import { NavigationContainer } from "@react-navigation/native";
import { createNativeStackNavigator } from "@react-navigation/native-stack";
import HomeScreen from "./src/screens/HomeScreen";
import ModuleScreen from "./src/screens/ModuleScreen";
import LessonScreen from "./src/screens/LessonScreen";
import QuizScreen from "./src/screens/QuizScreen";
import ProfileScreen from "./src/screens/ProfileScreen";

const Stack = createNativeStackNavigator();

export default function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator initialRouteName="Home" screenOptions={{ headerShown: false }}>
        <Stack.Screen name="Home" component={HomeScreen} />
        <Stack.Screen name="Module" component={ModuleScreen} />
        <Stack.Screen name="Lesson" component={LessonScreen} />
        <Stack.Screen name="Quiz" component={QuizScreen} />
        <Stack.Screen name="Profile" component={ProfileScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}
import React from "react";
import { View, Text, FlatList, TouchableOpacity, StyleSheet } from "react-native";
import lessons from "../data/lessons.json";

export default function HomeScreen({ navigation }: any) {
  const modules = Array.from(new Set(lessons.map((l) => l.module)));

  return (
    <View style={styles.container}>
      <Text style={styles.title}>SAFE MIND</Text>
      <Text style={styles.subtitle}>AI Safety Learning for Teens</Text>
      <FlatList
        data={modules}
        keyExtractor={(item) => item}
        renderItem={({ item }) => (
          <TouchableOpacity
            style={styles.card}
            onPress={() => navigation.navigate("Module", { moduleId: item })}
          >
            <Text style={styles.cardTitle}>{item}</Text>
            <Text style={styles.cardText}>Tap to begin</Text>
          </TouchableOpacity>
        )}
      />
      <TouchableOpacity onPress={() => navigation.navigate("Profile")}>
        <Text style={styles.profile}>View Profile →</Text>
      </TouchableOpacity>
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, backgroundColor: "#0f172a", padding: 20, paddingTop: 60 },
  title: { fontSize: 28, fontWeight: "700", color: "white" },
  subtitle: { fontSize: 14, color: "#e2e8f0", marginBottom: 20 },
  card: {
    backgroundColor: "#1f2937",
    padding: 16,
    borderRadius: 14,
    marginBottom: 12,
  },
  cardTitle: { fontSize: 18, color: "white", fontWeight: "600" },
  cardText: { fontSize: 12, color: "#94a3b8" },
  profile: { marginTop: 12, color: "#38bdf8" },
});
import React from "react";
import { View, Text, FlatList, TouchableOpacity, StyleSheet } from "react-native";
import lessons from "../data/lessons.json";

export default function ModuleScreen({ route, navigation }: any) {
  const { moduleId } = route.params;
  const filtered = lessons.filter((l) => l.module === moduleId);

  return (
    <View style={styles.container}>
      <Text style={styles.title}>{moduleId}</Text>
      <FlatList
        data={filtered}
        keyExtractor={(item) => item.id}
        renderItem={({ item }) => (
          <TouchableOpacity
            style={styles.card}
            onPress={() => navigation.navigate("Lesson", { lessonId: item.id })}
          >
            <Text style={styles.cardTitle}>{item.title}</Text>
            <Text style={styles.cardText}>{item.description}</Text>
          </TouchableOpacity>
        )}
      />
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, backgroundColor: "#0f172a", padding: 20, paddingTop: 60 },
  title: { fontSize: 24, fontWeight: "700", color: "white", marginBottom: 12 },
  card: { backgroundColor: "#1f2937", padding: 16, borderRadius: 14, marginBottom: 12 },
  cardTitle: { fontSize: 16, color: "white" },
  cardText: { fontSize: 12, color: "#94a3b8" },
});
import React from "react";
import { View, Text, ScrollView, StyleSheet, TouchableOpacity } from "react-native";
import lessons from "../data/lessons.json";

export default function LessonScreen({ route, navigation }: any) {
  const { lessonId } = route.params;
  const lesson = lessons.find((l) => l.id === lessonId);

  if (!lesson) {
    return (
      <View style={styles.container}>
        <Text style={styles.title}>Lesson not found</Text>
      </View>
    );
  }

  return (
    <ScrollView style={styles.container}>
      <Text style={styles.module}>{lesson.module}</Text>
      <Text style={styles.title}>{lesson.title}</Text>
      <Text style={styles.body}>{lesson.content}</Text>
      <TouchableOpacity
        style={styles.quizButton}
        onPress={() => navigation.navigate("Quiz", { lessonId })}
      >
        <Text style={styles.quizText}>Take Quiz</Text>
      </TouchableOpacity>
    </ScrollView>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, backgroundColor: "#0f172a", padding: 20, paddingTop: 60 },
  module: { color: "#38bdf8", fontWeight: "500", marginBottom: 6 },
  title: { fontSize: 22, color: "white", fontWeight: "700", marginBottom: 10 },
  body: { color: "#e2e8f0", fontSize: 14, lineHeight: 20 },
  quizButton: {
    marginTop: 20,
    backgroundColor: "#38bdf8",
    padding: 12,
    borderRadius: 10,
  },
  quizText: { textAlign: "center", color: "#0f172a", fontWeight: "700" },
});
import React, { useState } from "react";
import { View, Text, TouchableOpacity, StyleSheet } from "react-native";
import lessons from "../data/lessons.json";

export default function QuizScreen({ route, navigation }: any) {
  const { lessonId } = route.params;
  const lesson = lessons.find((l) => l.id === lessonId);
  const questions = lesson?.quiz || [];
  const [index, setIndex] = useState(0);
  const [score, setScore] = useState(0);
  const q = questions[index];

  const handleAnswer = (opt: string) => {
    if (opt === q.correct) setScore((s) => s + 1);
    if (index + 1 < questions.length) {
      setIndex((i) => i + 1);
    } else {
      navigation.replace("Profile", { lastScore: score + (opt === q.correct ? 1 : 0) });
    }
  };

  if (!lesson) {
    return (
      <View style={styles.container}>
        <Text style={styles.title}>Quiz not found</Text>
      </View>
    );
  }

  return (
    <View style={styles.container}>
      <Text style={styles.title}>{lesson.title} – Quiz</Text>
      <Text style={styles.question}>{q.question}</Text>
      {q.options.map((o: string) => (
        <TouchableOpacity key={o} style={styles.answer} onPress={() => handleAnswer(o)}>
          <Text style={styles.answerText}>{o}</Text>
        </TouchableOpacity>
      ))}
      <Text style={styles.progress}>
        {index + 1} / {questions.length}
      </Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, backgroundColor: "#0f172a", padding: 20, paddingTop: 80 },
  title: { fontSize: 20, color: "white", marginBottom: 20 },
  question: { fontSize: 16, color: "#e2e8f0", marginBottom: 12 },
  answer: { backgroundColor: "#1f2937", padding: 14, borderRadius: 10, marginBottom: 10 },
  answerText: { color: "white" },
  progress: { marginTop: 20, color: "#94a3b8" },
});
[
  {
    "id": "m1-l1",
    "module": "Module 1: What Is AI?",
    "title": "AI = Data + Pattern + Prediction",
    "description": "How machines learn from examples.",
    "content": "AI systems learn from data. They don't 'understand' like humans. They detect patterns and make predictions...",
    "quiz": [
      {
        "question": "What does AI mainly learn from?",
        "options": ["Data", "Magic", "Randomness", "Luck"],
        "correct": "Data"
      }
    ]
  },
  {
    "id": "m2-l1",
    "module": "Module 2: Digital Responsibility",
    "title": "You Are the Data",
    "description": "What you post, like, and watch trains AI.",
    "content": "Every click, like, or post becomes a data point...",
    "quiz": [
      {
        "question": "Why is privacy important?",
        "options": [
          "Because it keeps your data from being misused.",
          "Because it makes apps slower.",
          "Because school says so.",
          "Because it deletes the internet."
        ],
        "correct": "Because it keeps your data from being misused."
      }
    ]
  }
]
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {

    // student progress
    match /progress/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }

    // public lessons
    match /lessons/{doc} {
      allow read: if true;
      allow write: if false;
    }

    // certificates (readable by user + educators)
    match /certificates/{certId} {
      allow read: if request.auth != null;
      allow write: if request.auth != null && request.auth.token.admin == true;
    }
  }
}
import * as functions from "firebase-functions";
import * as admin from "firebase-admin";
import crypto from "crypto";
import fetch from "node-fetch";

admin.initializeApp();

export const issueCredential = functions.https.onCall(async (data, context) => {
  if (!context.auth) {
    throw new functions.https.HttpsError("unauthenticated", "Login required.");
  }
  const uid = context.auth.uid;

  // 1. get progress
  const progressRef = admin.firestore().collection("progress").doc(uid);
  const snap = await progressRef.get();
  const progress = snap.data() || {};

  if (!progress.completedModules || progress.completedModules.length < 6) {
    throw new functions.https.HttpsError("failed-precondition", "All modules not completed.");
  }

  // 2. make hash
  const payload = {
    uid,
    completedModules: progress.completedModules,
    ts: Date.now()
  };
  const hash = crypto.createHash("sha256").update(JSON.stringify(payload)).digest("hex");

  // 3. store certificate
  await admin.firestore().collection("certificates").doc(uid).set({
    uid,
    hash,
    ts: admin.firestore.FieldValue.serverTimestamp(),
    status: "PENDING_ONCHAIN"
  });

  // 4. (optional) call solana service
  try {
    await fetch(process.env.SOLANA_ENDPOINT as string, {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ root: hash, meta: payload })
    });
  } catch (e) {
    console.error("Solana anchor failed", e);
  }

  return { hash };
});
import {
  Connection,
  Keypair,
  PublicKey,
  Transaction,
  TransactionInstruction,
  sendAndConfirmTransaction,
} from "@solana/web3.js";

const RPC_URL = process.env.SOLANA_RPC || "https://api.devnet.solana.com";
const MEMO_PROGRAM_ID = new PublicKey("MemoSq4gqABAXKb96qnH8TysNcWxMyWCqXgDLGmfcHr");

// load keypair from local file/env – for demo we generate
const payer = Keypair.generate();

async function anchorRoot(root: string, metadata: any = {}) {
  const connection = new Connection(RPC_URL, "confirmed");

  const memoData = JSON.stringify({
    root,
    meta: metadata,
    app: "SAFE_MIND",
    v: 1,
  });

  const ix = new TransactionInstruction({
    keys: [],
    programId: MEMO_PROGRAM_ID,
    data: Buffer.from(memoData, "utf8"),
  });

  const tx = new Transaction().add(ix);
  tx.feePayer = payer.publicKey;
  tx.recentBlockhash = (await connection.getLatestBlockhash()).blockhash;

  const sig = await sendAndConfirmTransaction(connection, tx, [payer]);
  console.log("Anchored root on Solana:", sig);
  return sig;
}

// if run directly
if (require.main === module) {
  const root = process.argv[2] || "demo-root-hash";
  anchorRoot(root, { ts: Date.now() }).catch(console.error);
}

export { anchorRoot };
# Educator Guide – SAFE MIND

**Audience:** Middle school, high school, youth programs, faith-based youth groups.

## How to Use
1. Create teacher accounts.
2. Assign modules in order (1 → 6).
3. Students complete lessons and quizzes in-app.
4. Review student reflections in dashboard.
5. Download / verify completion certificate (hash).

## Assessment
- 60% module quizzes
- 20% reflections
- 20% final project (AI Bill of Rights)

## Safety
- All student data is private by default.
- No ads, no tracking.
- Students can export their learning record.
