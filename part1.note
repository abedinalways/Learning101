# OOP (Object-Oriented Programming) - সহজ বাংলায়, JavaScript দিয়ে

## OOP আসলে কী?

OOP মানে হলো তোমার কোডকে **object** হিসেবে সাজানো — যেখানে প্রতিটা object-এর নিজস্ব **ডেটা (properties)** আর **কাজ (methods)** থাকে। বাস্তব জীবনের জিনিস দিয়ে চিন্তা করলে সহজ হয়:

একটা **"Car"** যদি real-life object হয়, তাহলে:
- তার **properties**: color, brand, speed
- তার **methods (কাজ)**: start(), stop(), accelerate()

Frontend-এ তুমি যখন একটা `Button`, `Modal`, বা `ApiService` বানাও — এগুলাও আসলে object-ই, শুধু তুমি হয়তো খেয়াল করোনি।

---

## কেন OOP শিখব? (Frontend দৃষ্টিকোণ থেকে)

- কোড **reusable** হয় (একবার লিখো, বারবার ব্যবহার করো)
- কোড **organized** থাকে — বড় প্রজেক্টে (তোমার Traec বা Pixelstack-এর মতো) হাজারো ফাইল থাকলেও গুছানো থাকে
- **Bug কমে** — কারণ একটা object-এর ভেতরের ডেটা বাইরে থেকে random change হতে পারে না
- Team-এ কাজ করা সহজ হয় — সবাই বোঝে কোন class কী করে

---

## OOP-এর ৪টা মূল স্তম্ভ (Pillars)

### ১. Encapsulation (ডেটা লুকিয়ে রাখা)

মানে হলো — একটা object-এর ভেতরের ডেটা বাইরে থেকে সরাসরি access/change করতে না দেওয়া। শুধু নির্দিষ্ট method দিয়েই সেই ডেটা change করা যাবে।

```javascript
class ApiService {
  #baseUrl; // # দিয়ে private property - বাইরে থেকে access করা যাবে না

  constructor(baseUrl) {
    this.#baseUrl = baseUrl;
  }

  // শুধু এই method দিয়েই baseUrl জানা যাবে
  getBaseUrl() {
    return this.#baseUrl;
  }

  async fetchData(endpoint) {
    const res = await fetch(`${this.#baseUrl}/${endpoint}`);
    return res.json();
  }
}

const api = new ApiService("https://api.example.com");
api.fetchData("users");
console.log(api.#baseUrl); // ❌ Error! বাইরে থেকে access করা যাবে না
```

**Daily use:** তোমার `baseApi.ts` কনফিগারেশনে যদি টোকেন বা সেনসিটিভ ডেটা থাকে, সেগুলো `#private` field-এ রাখলে accidental change/leak হবে না।

---

### ২. Abstraction (জটিলতা লুকিয়ে সহজ interface দেওয়া)

User-কে শুধু দরকারি জিনিসটা দেখাও, ভেতরের জটিল লজিক লুকিয়ে রাখো।

```javascript
class FormValidator {
  validate(formData) {
    // ভেতরে অনেক জটিল রেগেক্স/লজিক আছে, কিন্তু বাইরে থেকে শুধু validate() call করলেই হয়
    return this.#checkEmail(formData.email) && this.#checkPassword(formData.password);
  }

  #checkEmail(email) {
    return /\S+@\S+\.\S+/.test(email);
  }

  #checkPassword(password) {
    return password.length >= 8;
  }
}

const validator = new FormValidator();
validator.validate({ email: "test@test.com", password: "12345678" }); // simple!
```

**Daily use:** তুমি যখন একটা `useForm` hook বা `Modal` component বানাও, ইউজার শুধু props পাঠায় (`isOpen`, `onClose`) — ভেতরে animation/focus-trap কীভাবে হচ্ছে সেটা জানার দরকার নেই।

---

### ৩. Inheritance (একটা class থেকে আরেকটা class-এর বৈশিষ্ট্য পাওয়া)

একটা "parent" class-এর সব property/method আরেকটা "child" class পেয়ে যায়, আর দরকার হলে নিজের মতো করে বদলাতে পারে।

```javascript
class Notification {
  constructor(message) {
    this.message = message;
  }

  show() {
    console.log(`Notification: ${this.message}`);
  }
}

class ErrorNotification extends Notification {
  constructor(message) {
    super(message); // parent constructor call
  }

  show() {
    console.log(`❌ Error: ${this.message}`); // parent-এর method override করলাম
  }
}

class SuccessNotification extends Notification {
  show() {
    console.log(`✅ Success: ${this.message}`);
  }
}

new ErrorNotification("Login failed").show();
new SuccessNotification("Profile updated").show();
```

**Daily use:** ধরো তোমার একটা base `HttpService` class আছে যেখানে common GET/POST logic আছে। এখন `UserService extends HttpService` আর `ProductService extends HttpService` লিখলে বারবার একই fetch logic লিখতে হবে না।

---

### ৪. Polymorphism (একই নামের method, ভিন্ন ভিন্ন behavior)

উপরের `show()` method-টাই polymorphism-এর উদাহরণ — একই নামের method, কিন্তু প্রতিটা class-এ আলাদা কাজ করছে।

```javascript
const notifications = [
  new ErrorNotification("Something broke"),
  new SuccessNotification("All good"),
];

notifications.forEach((n) => n.show()); 
// একই .show() call করলাম, কিন্তু আউটপুট আলাদা আলাদা
```

**Daily use:** Order status stepper বানানোর সময় (যেটা তুমি সম্প্রতি করছিলে) — `Pending`, `Shipped`, `Delivered` স্টেপের জন্য আলাদা class বানিয়ে প্রতিটায় নিজস্ব `renderIcon()` method রাখতে পারো, একই ইন্টারফেসে।

---

## Real-life Frontend Example (সব একসাথে)

```javascript
// Abstraction + Encapsulation
class HttpService {
  #baseUrl;

  constructor(baseUrl) {
    this.#baseUrl = baseUrl;
  }

  async get(endpoint) {
    const res = await fetch(`${this.#baseUrl}${endpoint}`);
    if (!res.ok) throw new Error("Request failed");
    return res.json();
  }

  async post(endpoint, data) {
    const res = await fetch(`${this.#baseUrl}${endpoint}`, {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify(data),
    });
    return res.json();
  }
}

// Inheritance
class UserService extends HttpService {
  constructor() {
    super("https://api.traec.pixelstack.cloud/");
  }

  getProfile(id) {
    return this.get(`users/${id}`);
  }

  updateProfile(id, data) {
    return this.post(`users/${id}`, data);
  }
}

// ব্যবহার
const userService = new UserService();
userService.getProfile(1).then((user) => console.log(user));
```

এইভাবে তুমি চাইলে `OrderService`, `AuthService`ও একই `HttpService` থেকে extend করে বানাতে পারবে — GET/POST logic বারবার লিখতে হবে না।

---

## Daily Code-এ কীভাবে Implement করবে (practical tips)

1. **যখনই একই ধরনের logic ২-৩ জায়গায় repeat করছো** — সেটাকে class-এ নিয়ে আসো (যেমন সব API call-এর জন্য base class)।
2. **Sensitive/internal data** সবসময় `#private` field-এ রাখো, শুধু method দিয়ে expose করো।
3. **React-এ সরাসরি class কম লাগে** (Hooks-এর যুগে), কিন্তু **utility/service layer**-এ (API calls, form validators, state managers) OOP খুব কাজে লাগে।
4. Component design করার আগে চিন্তা করো — "এটা কি reusable base থেকে extend করা যায়?"

> Tip: React component নিজে class না বানিয়ে function-ই রাখো (এখন এটাই standard practice), কিন্তু তার পেছনের business logic (services, validators, formatters) class দিয়ে organize করতে পারো।

---

## Interview-এ OOP নিয়ে কীভাবে উত্তর দেবে

### সাধারণ প্রশ্ন: "What is OOP?"
**উত্তর:** "OOP is a programming paradigm where we organize code around objects that bundle data and behavior together. It's built on four pillars — Encapsulation, Abstraction, Inheritance, and Polymorphism — which help make code more modular, reusable, and easier to maintain."

### "Explain Encapsulation with example"
জাভাস্ক্রিপ্টের `#private` field-এর উদাহরণ দাও (উপরে যেটা করলাম)। বলো: "It hides internal implementation details and only exposes what's necessary through public methods."

### "Difference between Abstraction and Encapsulation?"
- **Encapsulation** = ডেটা bundle করে হাইড করা (how)
- **Abstraction** = জটিলতা লুকিয়ে সহজ interface দেওয়া (what)

সহজ কথায়: Encapsulation হলো "লুকানো", Abstraction হলো "সরল করা"।

### "What is the difference between class-based and prototype-based OOP in JS?"
বলো: "JavaScript is prototype-based under the hood — every object inherits from another object via the prototype chain. The `class` syntax introduced in ES6 is just syntactic sugar over prototypal inheritance, making it more readable and similar to classical OOP languages."

### "Why use OOP in a React/Next.js project if components are functional?"
এইটা তোমার জন্য গুরুত্বপূর্ণ প্রশ্ন হতে পারে:
> "Even though React components are functional, OOP principles are still valuable in the service/business-logic layer — like API clients, validators, or state managers — where inheritance and encapsulation help avoid code duplication and keep logic organized."

### Common follow-up: "Can you give a real example from your project?"
তোমার নিজের অভিজ্ঞতা থেকে বলতে পারো:
> "In one of my projects, I built a base `HttpService` class with common GET/POST logic, then extended it for `UserService` and `OrderService`, so each service only had to define its specific endpoints instead of repeating fetch logic."

---

## Quick Recap (এক নজরে)

| Pillar | মানে কী | JS-এ কীভাবে |
|---|---|---|
| Encapsulation | ডেটা লুকিয়ে রাখা | `#privateField` |
| Abstraction | জটিলতা লুকিয়ে সহজ interface | public methods only |
| Inheritance | parent থেকে পাওয়া | `extends`, `super()` |
| Polymorphism | একই নাম, ভিন্ন behavior | method override |

চর্চার জন্য পরামর্শ: তোমার existing কোনো একটা API service ফাইল (যেমন `providerServiceApi.ts`) নিয়ে চেষ্টা করো এটাকে class-based structure-এ রিফ্যাক্টর করতে — practice-এর সবচেয়ে ভালো উপায় হলো নিজের কোডে apply করা।
