# 🎮 Game Glitch Investigator: The Impossible Guesser

## 🚨 The Situation

You asked an AI to build a simple "Number Guessing Game" using Streamlit.
It wrote the code, ran away, and now the game is unplayable. 

- You can't win.
- The hints lie to you.
- The secret number seems to have commitment issues.

## 🛠️ Setup

1. Install dependencies: `pip install -r requirements.txt`
2. Run the broken app: `python -m streamlit run app.py`

## 🕵️‍♂️ Your Mission

1. **Play the game.** Open the "Developer Debug Info" tab in the app to see the secret number. Try to win.
2. **Find the State Bug.** Why does the secret number change every time you click "Submit"? Ask ChatGPT: *"How do I keep a variable from resetting in Streamlit when I click a button?"*
3. **Fix the Logic.** The hints ("Higher/Lower") are wrong. Fix them.
4. **Refactor & Test.** - Move the logic into `logic_utils.py`.
   - Run `pytest` in your terminal.
   - Keep fixing until all tests pass!

## 📝 My Experience

**Purpose:** A number guessing game built with Streamlit. The player picks a difficulty (Easy 1–20, Normal 1–100, Hard 1–50), then guesses a secret number within a limited number of attempts. Hints say whether to go higher or lower, and the score changes with each guess.

**Bugs found:**
- The hints were backwards: a guess above the secret said "Go HIGHER!" and a guess below said "Go LOWER!".
- The New Game button could not restart a finished game. It reset `attempts` and `secret` but not `status`, so the "game over" guard stopped the script after every rerun.
- New Game also ignored the difficulty range (it always used 1–100), did not reset the score or guess history, and started with a different attempt count than a fresh game. Its success message was never shown because `st.rerun()` ran right after it.

**Fixes applied:**
- Moved `check_guess` into `logic_utils.py`, imported it in `app.py`, and corrected the hint messages (including the `TypeError` fallback).
- Rewrote the New Game handler to reset `status`, `score`, `history` and `attempts`, pick the secret from the difficulty range, clear the guess box, and show "New game started." after the rerun.
- Added pytest cases for hint direction and fixed the existing tests to unpack the `(outcome, message)` tuple.

**Still unfixed:** on even-numbered attempts `app.py` converts the secret to a string before calling `check_guess`, so an exact guess cannot win on those attempts and hints can be wrong. The info box also always says "between 1 and 100" regardless of difficulty.

## 📸 Demo Walkthrough

Describe your fixed game in numbered steps so a reader can follow along without watching a video:

1. Choose your desired game difficulty
2. Start a game by making a guess in the text box (e.g, 5)
3. If you guessed incorrectly, keep guessing until you get the correct number, or if you run out of guesses.
4. Game returns hints for each guess, if toggled on.
5. Use the New Game button to start a new game.

## 🧪 Test Results

$ pytest tests/ -v
============================= test session starts ==============================
collected 6 items

tests/test_game_logic.py::test_winning_guess PASSED                      [ 16%]
tests/test_game_logic.py::test_guess_too_high PASSED                     [ 33%]
tests/test_game_logic.py::test_guess_too_low PASSED                      [ 50%]
tests/test_game_logic.py::test_too_high_guess_tells_player_to_go_lower PASSED [ 66%]
tests/test_game_logic.py::test_too_low_guess_tells_player_to_go_higher PASSED [ 83%]
tests/test_game_logic.py::test_hint_direction_with_string_secret PASSED  [100%]

============================== 6 passed in 0.01s ===============================


