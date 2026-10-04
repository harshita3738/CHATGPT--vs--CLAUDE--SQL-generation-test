# My First Tech Experiment: Comparing ChatGPT vs. Claude for SQL Generation

**Author:** Harshita Jain  
**Date:** October 2026  

---

### Abstract
When you are learning data analytics or working on coding projects, AI tools feel like a superpower. But as a student or independent developer, how do you know which AI to trust when things get complicated? Most tech reviews focus only on whether an AI writes the right code syntax. In this study, I wanted to look deeper. I ran a controlled experiment putting OpenAI's ChatGPT and Anthropic's Claude head-to-head using a tricky multi-table SQL database problem. I wanted to see if they could handle a hidden data preservation trap. To my surprise, while both models tied perfectly on the core coding logic (`LEFT JOIN` and `COALESCE`), they treated me completely differently as a user. ChatGPT blew me away by automatically creating a simulated data output table to prove its code worked, making it perfect for visual learners. Claude, on the other hand, acted like a strict programmer—giving me brilliant text notes and a sorted list, but completely skipping the visual proof. 

---

### 1. Introduction: Why I Ran This Test
A few weeks ago, I started looking into how Artificial Intelligence is changing database work. Today, anyone can open up a chat interface, type what they want in plain English, and watch a Structured Query Language (SQL) script appear instantly. It feels easy, but as I dug deeper, I realized how dangerous it is to blindly trust these tools. In real business tracking, a single logical mistake—like using an `INNER JOIN` instead of a `LEFT JOIN`—can silently delete hundreds of customer accounts from a final financial spreadsheet. 

I wanted to find out which AI is truly the better assistant for someone building their skills with zero corporate experience. Is it just about writing code that doesn't crash? Or does the way an AI explains things and shows its work matter more? To find out, I set up a direct pop quiz for both ChatGPT and Claude to see where they shine and where they trip up.

---

### 2. My Methodology: Setting Up the Trap
To keep things fair, I opened both ChatGPT and Claude in separate tabs at the exact same time. I designed a fake, classic business database with two tables:
*   **The `Users` Table:** A basic customer directory tracking `user_id`, `name`, and `country`.
*   **The `Purchases` Table:** A standard ledger tracking `purchase_id`, `user_id`, `amount`, and `order_date`.

I intentionally designed the question with a logical hurdle. I asked the tools to calculate total spending but insisted that users who have spent absolutely nothing must still show up in the final list with a zero. Here is the exact prompt I copied and pasted into both search bars:

> *"I have two database tables: Users (columns: user_id, name, country) and Purchases (columns: purchase_id, user_id, amount, order_date). Write a SQL query that shows the name of each user and the total amount of money they spent. Make sure users who haven't bought anything yet are still listed with a total of 0."*

To get a perfect score from me, the AI models had to do two specific things: use a non-restrictive `LEFT JOIN` so inactive users wouldn't get erased, and wrap the math inside a null-handling function like `COALESCE` to turn blank database spaces into a clean number `0`.

---

### 3. What the Code Showed: A Technical Tie
When the responses loaded, I was genuinely impressed. Neither tool fell into the common beginner trap of using a standard matching join that destroys empty rows. They both nailed the logic.

#### The ChatGPT Script:
```sql
SELECT
    u.name,
    COALESCE(SUM(p.amount), 0) AS total_spent
FROM Users u
LEFT JOIN Purchases p
    ON u.user_id = p.user_id
GROUP BY u.user_id, u.name;
```

#### The Claude Script:
```sql
SELECT
    u.name,
    COALESCE(SUM(p.amount), 0) AS total_spent
FROM Users u
LEFT JOIN Purchases p
    ON u.user_id = p.user_id
GROUP BY u.user_id, u.name
ORDER BY total_spent DESC;
```

Looking at the code side-by-side, they are almost identical. Both engines correctly used `FROM Users u LEFT JOIN Purchases p`, making the `Users` table the primary master list. They also both knew that users without orders would return blank `NULL` rows, which they cleanly intercepted using `COALESCE(SUM(p.amount), 0)`. 

I also noticed an awesome security habit in both models: they both grouped the data by `u.user_id` alongside `u.name`. As a learner, this taught me something great: if a database has two entirely different customers named "Rahul," grouping by the unique ID keeps their money records from blending together. The only minor difference was that Claude added an extra `ORDER BY total_spent DESC` clause at the end to automatically sort the highest spenders first—a very practical real-world touch.

---

### 4. The Shocking Twist: Visual Presentation vs. Text Blurbs
The real shocker happened *after* the code blocks. While their coding skills were a perfect tie, the way the two systems communicated with me as a human user was night and day. 

#### ChatGPT's Visual Edge
Right underneath its explanation, ChatGPT automatically built a mock database output table out of nowhere to show me exactly what my final data rows would look like:

```
name            total_spent
Rahul           1500
Priya           3200
Amit            0
Neha            850
```

This single feature completely won me over. By showing a physical row where a user named "Amit" sits with a total of `0`, ChatGPT instantly proved to my eyes that its logic worked. For a student like me who doesn't have an expensive database server running on their laptop to test queries, this visual confirmation makes learning incredibly simple and intuitive.

#### Claude's Programmatic Focus
Claude took a totally different path. It gave me a phenomenal, incredibly detailed text breakdown of its code logic. It explained the primary keys and grouping rules beautifully. But it stopped right there. It didn't provide a mock output table at all. During my experiment, I realized that if a user wants to see a visual example of the data in Claude, they are forced to write a second prompt explicitly asking for it. 

---

### 5. My Final Verdict
This experiment proved to me that while ChatGPT and Claude are equally smart when writing raw SQL code, they are built for entirely different mindsets. 

Claude operates like a precise, developer-first tool. It gives you incredible written notes and useful code additions like sorting, but it assumes you already know what you are doing. ChatGPT, on the other hand, feels like an amazing personal tutor. By automatically generating mock output tables, it gives you the immediate visual proof you need to build your confidence and check your logic. 

For independent researchers and beginners trying to enter the tech field with zero professional experience, ChatGPT’s ability to *show* the data, rather than just explain it, makes it my top recommendation as a coding assistant.

---

### 6. Acknowledgments & References
I want to thank the engineering teams behind OpenAI's ChatGPT and Anthropic's Claude, which served as the live subjects for my test experiment. I also want to acknowledge the data communities on GitHub and Kaggle for providing the open educational schema inspirations that helped me build this testing methodology.
*   Silberschatz, A. (2024). *Database System Concepts*. McGraw-Hill.
*   OpenAI Research Blog. (2026). *Advancements in Context-Aware Code Generation and UI Layouts*.
*   Anthropic Data Insights. (2026). *Evaluating Structural Query Precision Across Large Language Models*.
