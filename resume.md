---
layout: post
title: Resume
permalink: /resume/
---

**Krish Kumar** — Greater Noida, UP
[krishkumarmath@gmail.com](mailto:krishkumarmath@gmail.com) · [GitHub: Krishkumar6](https://github.com/Krishkumar6) · LeetCode: 500+ problems solved · [Blog: krishkumar6.github.io](/)

## Summary

B.Tech graduate in Electronics and Communication Engineering with hands-on experience in Java Full-Stack development, System Design, and AI/ML (LLMs, RAG). Self-driven builder who has independently shipped multiple full-stack projects from design to deployment, with 500+ problems solved on LeetCode. Eager to contribute ideas and build impactful software in a fast-moving team.

## Technical Skills

| | |
|---|---|
| **Backend** | Java, Spring Boot, Spring MVC, Spring Security, Hibernate, JPA, REST API |
| **Frontend** | React.js, JavaScript, HTML, CSS |
| **Machine Learning** | Python, FastAPI, scikit-learn, Pandas |
| **AI / LLM** | LLMs, RAG, Vector Databases (ChromaDB), LangChain, Embeddings |
| **Database** | MySQL, SQL |
| **Auth & Security** | JWT Authentication, Role-Based Access Control |
| **Tools** | Git, GitHub, Postman, IntelliJ IDEA, VS Code |
| **System Design** | Scalability, Load Balancing, Consistent Hashing, Caching, Database Sharding & Replication, Message Queues, Microservices, CAP Theorem, Rate Limiting |

## Projects

### Smart Healthcare Appointment & Risk Prediction System
*Spring Boot, React.js, Python (FastAPI), MySQL, Spring Security, JWT, REST API*

- Architected a microservice-based healthcare platform with a Spring Boot REST API and a separate Python/FastAPI service exposing a scikit-learn logistic regression model that predicts patient no-show risk from booking history, lead time, and distance.
- Implemented JWT-based authentication and role-based access control (Patient, Doctor, Admin) with Spring Security; built a symptom-to-department routing feature that classifies free-text patient input to auto-assign the correct specialist.
- Built a risk-prioritized doctor dashboard in React.js sorting appointments by no-show probability, and a scheduled reminder job (Twilio SMS integration) that targets only high-risk patients to reduce alert fatigue.

### Banking Management System
*Spring Boot, React.js, MySQL, Hibernate/JPA, Spring Security, REST API*

- Developed a secure full-stack banking application with Spring Boot REST APIs handling account creation, fund transfers, deposits, and withdrawals with server-side validation and atomic balance updates.
- Implemented transaction history tracking and account statement generation using Hibernate/JPA with MySQL; secured all endpoints via Spring Security with authenticated session management.
- Built a clean React.js dashboard displaying live account balance, recent transactions, and seamless transfer functionality via async API calls.

### Rental Agreement Intelligence Assistant
*Python, FastAPI, SQLAlchemy, ChromaDB, Hugging Face, Groq, BeautifulSoup*

- Built a FastAPI backend that scrapes listing data via BeautifulSoup and extracts structured lease fields (rent, deposit, dates, pet policy) from raw text using an LLM in JSON mode, validated with Pydantic and persisted via SQLAlchemy.
- Built a RAG pipeline embedding lease text via the Hugging Face API into a Chroma vector store, with tenant-scoped metadata filtering so one tenant's agent can never retrieve another tenant's lease clauses.
- Designed a tool-calling LLM agent that decides whether to search lease clauses, fetch agreement fields, or fetch listing details before answering, grounding every response in retrieved data rather than model memory.

## Education

| | |
|---|---|
| **G.L. Bajaj Institute of Technology and Management** — B.Tech in Electronics and Communication Engineering | Nov 2022 – May 2026 |
| **Jaypee Vidya Mandir, CBSE** — Senior Secondary (PCM), 80% | March 2021 |
| **Jagdish Memorial Public School, CBSE** — Secondary, 82% | March 2020 |

## Achievements

- Solved 500+ DSA problems on LeetCode in Java, covering Arrays, Trees, Graphs, DP and more.
- Self-learned Spring Boot, Hibernate, JPA, React.js, and applied ML microservice integration.
- Maintain a personal engineering blog ([krishkumar6.github.io](/)) writing notes on system design and software engineering concepts.
</content>
