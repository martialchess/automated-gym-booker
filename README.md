# Automated Gym Class Booker

This project automates the process of logging into a gym website, booking specific classes (Tuesday/Thursday 6pm), and verifying those bookings using Selenium.

---

## Project Overview

The goal of this project is to:
- Log in to the gym website automatically.
- Book all available Tuesday and Thursday 6:00 PM classes (and join waitlists if needed).
- Verify that the bookings appear on the "My Bookings" page.
- Provide robust retry logic for network or UI issues.

---

## Development Journey & Approaches

### 1. **Initial Complex Logic**

- **Day and Time Matching:**  
  We started by matching classes using a combination of text, IDs, and attributes.  
  We tried to extract day and time using both visible text and element IDs, which led to fragile code that broke with minor HTML changes.

- **Element Navigation:**  
  Used complex XPath and CSS selectors to traverse between class cards and their parent day groups.  
  This sometimes failed when the page structure changed or when elements loaded slowly.

- **State Handling:**  
  Early attempts tried to handle every possible booking state (booked, waitlisted, available, etc.) with nested if-else logic, making the code hard to read and maintain.

- **Retry Logic:**  
  We experimented with retry wrappers for both login and booking, but sometimes passed arguments incorrectly, causing unexpected keyword argument errors.

- **Verification:**  
  Our first verification logic tried to match bookings using class names, dates, and times, but was brittle and failed if the page format changed or if bookings were already present.

---

### 2. **Problems Encountered**

- **Element Click Intercepted:**  
  Selenium often failed to click booking buttons because of overlays or sticky navigation bars.  
  We fixed this by scrolling buttons into view and offsetting for sticky headers.

- **Timing Issues:**  
  The script sometimes ran before elements were loaded, causing `NoSuchElementException` or `TimeoutException`.  
  We added explicit waits and retry logic to handle slow page loads.

- **Bloated and Fragile Code:**  
  As we tried to handle every edge case, the code became bloated, hard to debug, and difficult to maintain.

- **Verification Mismatches:**  
  Our verification logic sometimes failed to find bookings that were clearly present, due to overly strict matching or incorrect selectors.

---

### 3. **Simplification and Refactoring**

- **Back to Boilerplate:**  
  After several failed attempts and debugging sessions, we reverted to a simplified, boilerplate approach:
    - Use clear, robust selectors for class cards and booking buttons.
    - Only match on essential information (day and time).
    - Use retry wrappers only where truly needed (login, booking, navigation).
    - Keep the booking and verification logic linear and easy to follow.

- **Clear Flow:**  
  The final code:
    1. Logs in with retry.
    2. Loops through all class cards, booking or waitlisting Tuesday/Thursday 6pm classes.
    3. Navigates to "My Bookings" and verifies the expected number of bookings.
    4. Prints clear logs and a summary.

- **Why This Worked:**  
  - The simplified logic is easier to debug and maintain.
  - Fewer moving parts means fewer places for bugs to hide.
  - Explicit waits and retries make the script robust to network and UI delays.

---

## Final Solution: Key Features

- **Robust Retry Logic:**  
  For login, booking, and navigation to handle network/UI flakiness.

- **Explicit Waits:**  
  Ensures elements are present and interactable before acting.

- **Clear, Minimal Selectors:**  
  Uses only what's necessary to identify days, times, and buttons.

- **Straightforward Verification:**  
  Counts and verifies bookings by matching only on day and time.

- **Readable Logs:**  
  Prints each action and a summary for easy debugging.

---

## How to Use

1. **Install requirements:**  
   ```bash
   pip install selenium
   ```

2. **Set your credentials:**  
   Edit `ACCOUNT_EMAIL` and `ACCOUNT_PASSWORD` in `main.py`.

3. **Run the script:**  
   ```bash
   python main.py
   ```

4. **Review the logs:**  
   The script will print booking actions and verification results.

---

## Lessons Learned

- **Keep it simple:**  
  Overly complex logic leads to fragile, hard-to-maintain code.
- **Use retries and waits:**  
  Network and UI delays are common; handle them gracefully.
- **Debug with clear logs:**  
  Print what the script is doing at each step for easy troubleshooting.
- **Revert when needed:**  
  Sometimes, going back to a simple, boilerplate approach is the best way forward.

---

## Credits

Developed by Rida Malik.  
Thanks to the App Brewery for the gym site and the Selenium community for robust automation tools.
