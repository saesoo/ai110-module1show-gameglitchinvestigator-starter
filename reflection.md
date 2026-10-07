# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

## 1. What was broken when you started?
The game wouldn't take in certain input buttons, like pressing "enter" to submit a guess was not working.
The hints that say "go lower" or "go higher" were inaccurate and throws the player off when guessing the actual answer.
Trying to start a new game does not work, and a new game cannot be started until you refresh the page.
It says game is over, even if theres still an attempt left.

- What did the game look like the first time you ran it?
- List at least two concrete bugs you noticed at the start  
  (for example: "the hints were backwards").

**Bug Reproduction Log**

Document at least 3 bugs you found. Add rows as needed.

| Input | Expected Behavior | Actual Behavior | Console Output / Error |
|-------|-------------------|-----------------|------------------------|
|   8   | Go Higher            Go Lower             none
|   6   | Go Higher            Go Lower             none
|  45   | Go Lower             Go Lower             none

---

## 2. How did you use AI as a teammate?

For this project, I used a mix of Gemini and Claude Code. Gemini was used to help me answer questions I had over the assignment instructions. Claude Code was used to help with debugging any glitches in the game, and helped me understand what and where the problems were. One example of an AI suggestion that was correct was for the backwards hint problem. 

      outcome examples: "Win", "Too High", "Too Low"
    """
    if guess == secret:
        return "Win", "🎉 Correct!"

    try:
        if guess > secret:
            return "Too High", "📉 Go LOWER!"
        else:
            return "Too Low", "📈 Go HIGHER!"
    except TypeError:
        g = str(guess)
        if g == secret:
            return "Win", "🎉 Correct!"
        if g > secret:
            return "Too High", "📉 Go LOWER!"
        return "Too Low", "📈 Go HIGHER!"
  I verified the result by making sure the code was properly running in the game UI and by using pytest to make sure the test cases were accounted for.
  One example of an AI suggestion I didn't accept was for the New Game Button. I didn't accept it because it was too messy. I asked it to compile cleaner formatted code that was still easy to understand.

- Which AI tools did you use on this project (for example: ChatGPT, Gemini, Copilot)?
- Give one example of an AI suggestion that was correct (including what the AI suggested and how you verified the result).
- Give one example of an AI suggestion you did not accept as written (including what the AI suggested, why you rejected or changed it, and how you verified your version). It does not have to be a suggestion that was wrong: over-engineered, out of scope, harder to read, or a poor fit for this codebase all count.

---

## 3. Debugging and testing your fixes

By using pytest, I was able to decide what bugs were fixed. Then I ran the application again on streamlit and verified that my inputs were getting the expected outputs. I asked Claude to create tests for me, which it also created different files like .pytest_cache, and ran all 6 tests for me, and they came out to be successful. 

- How did you decide whether a bug was really fixed?
- Describe at least one test you ran (manual or using pytest)  
  and what it showed you about your code.
- Did AI help you design or understand any tests? How?

---

## 4. What did you learn about Streamlit and state?

Streamlit is a way to run your code as a game. When ran, it creates a session for the user. The session is active when the user opens it. There is a stopping point for the session , and it's created when the code prompts that the game has ended or is over. Streamlit reruns your whole python script from top to bottom every time something happens. That includes clicking a button, typing in a box, or changing a dropdown. 

- How would you explain Streamlit "reruns" and session state to a friend who has never used Streamlit?

---

## 5. Looking ahead: your developer habits

I want to continue the habit of creating tests in my code to make sure my debugging was effective. 
I would use Claude a little differently. I want to try utilizing agents more or making it to where Claude asks for questions beforehand. This project showed me that AI-generated code can look polished and still be wrong in small ways, like backwards hints or a button that forgets to reset one variable. I now treat it as a first draft that I have to read, test and verify myself, not as something I can trust because it runs.

- What is one habit or strategy from this project that you want to reuse in future labs or projects?
  - This could be a testing habit, a prompting strategy, or a way you used Git.
- What is one thing you would do differently next time you work with AI on a coding task?
- In one or two sentences, describe how this project changed the way you think about AI generated code.
