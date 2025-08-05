# CompanAIon - Career Companion AI

Career Companion AI is an intelligent assistant designed to help students and early professionals make informed decisions about their future careers. It uses the **Gemini API** and integrates advanced AI techniques to provide **personalized career recommendations**, **key skills to develop**, **online course suggestions**, and **example job roles** based on a user’s background, interests, and goals.

This project demonstrates the real-world application of four key AI implementation concepts: **Prompting**, **Structured Output**, **Function Calling**, and **Retrieval-Augmented Generation (RAG)** all of which will be fully implemented in this system.

---

## Project Flow Summary

1. **User submits** educational background, interests, and goals.
2. A **dynamic prompt** is generated using this input and sent to Gemini.
3. Gemini provides an initial **structured response**.
4. Gemini can trigger **function calls** like `getCourses()` or `getJobs()`.
5. The backend fetches **real data using RAG** and returns a final enriched response.
6. Output is displayed in a structured frontend interface.

---

## Core Concepts & Their Implementation

### 1. Prompting

**What it is:** Prompting involves dynamically creating queries or instructions that are passed to the Gemini API.

**How it’s implemented:**  
When a user submits their profile (e.g., "2nd-year CS student interested in AI and Robotics"), the backend generates a tailored prompt like this:

```
Act as a career counselor. Based on the user’s background, suggest:
- 3 relevant career paths
- 5 skills to learn
- 3 online courses with URLs
- 2 job/internship roles

User Info:
Education: 2nd-year CS
Interests: AI, Robotics
Goals: Build smart robots or AI applications.
```

This ensures high-quality, user-specific responses.

---

### 2. Structured Output

**What it is:** Ensures the AI returns responses in a machine-readable, standardized format (e.g., JSON).

**How it’s implemented:**  
Gemini is instructed to return data in JSON format like this:

```json
{
  "career_paths": ["AI Researcher", "Robotics Engineer"],
  "skills_to_learn": ["Python", "ROS", "Deep Learning"],
  "recommended_courses": [
    {"title": "Deep Learning Specialization", "platform": "Coursera", "url": "https://..."},
    {"title": "ROS Basics", "platform": "Udemy", "url": "https://..."}
  ],
  "job_roles": ["AI Intern at OpenAI", "Robotics Intern at Boston Dynamics"]
}
```

This enables consistent rendering in the frontend and simplifies data handling.

---

### 3. Function Calling

**What it is:** Allows Gemini to request external data through explicit function calls.

**How it’s implemented:**  
Gemini can return a function call object like:

```json
{
  "function_call": {
    "name": "getCourses",
    "parameters": {
      "skills": ["ROS", "Python"]
    }
  }
}
```

The backend interprets this, runs the `getCourses(skills)` function, fetches real data from a course API or local DB, and returns the results.

Available Functions:
- `getCourses(skills)` – Returns relevant courses.
- `getJobs(role)` – Returns job suggestions.
- `getStats(careerPath)` – Returns job market stats (optional).

---

### 4. Retrieval-Augmented Generation (RAG)

**What it is:** Combines external data with language model responses to improve accuracy and relevance.

**How it’s implemented:**  
- A local database or API (Coursera, Udemy, etc.) will be queried using skills/interests.
- The retrieved data is either injected into the initial prompt or merged with Gemini’s final response.


**Example RAG flow:**
1. Gemini triggers `getCourses(skill: "Deep Learning")`.
2. Backend searches course DB and finds:
   - "Intro to Deep Learning", Coursera, https://...
   - "Practical AI", Udemy, https://...
3. These are inserted into the final JSON response shown to the user.

This makes the AI assistant grounded in **real-world, up-to-date information**.

---

## System Architecture

```
[Frontend - React]
     ⬇ User Input
[Backend - Node.js]
     ⬇ Prompt Construction (Prompting)
[Gemini API]
     ⬇ JSON Output / Function Call
[Backend executes function]
     ⬇ RAG (Query DB/API)
[Gemini Final Output]
     ⬇ UI Display
```

---

## Tech Stack

| Component   | Technology              |
|-------------|--------------------------|
| Frontend    | React + Tailwind CSS     |
| Backend     | Node.js + Express        |
| AI API      | Google Gemini API        |
| RAG Source  | Local JSON or APIs       |
| Deployment  | Vercel (Frontend), Render(Backend) |

---

## Example Final Output

```json
{
  "career_paths": ["ML Engineer", "AI Researcher"],
  "skills_to_learn": ["Python", "TensorFlow", "Statistics"],
  "recommended_courses": [
    {"title": "ML Foundations", "platform": "Coursera", "url": "https://..."},
    {"title": "AI Basics", "platform": "Udemy", "url": "https://..."}
  ],
  "job_roles": ["ML Intern at Google", "AI Intern at OpenAI"]
}
```

---

## Summary

| Concept            | Implementation Details |
|--------------------|------------------------|
| Prompting          | Dynamic prompt from user input |
| Structured Output  | JSON output from Gemini |
| Function Calling   | Gemini calls backend functions (e.g., getCourses) |
| RAG                | Backend retrieves real data (courses/jobs) and feeds it to Gemini |

---

