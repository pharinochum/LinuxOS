---
marp: false
---

# Lesson Plan: Linux Operating System - Bash Shell Scripting II: Advanced

**Course Title:** Linux Operating System  
**Module/Topic:** Bash Shell Scripting II: Advanced  
**Total Duration:** 2.5 hours (150 minutes)  

**Target Audience:** Intermediate learners familiar with basic Bash scripting (e.g., variables, conditionals, basic commands).  

**Prerequisites:** Basic knowledge of Linux terminal, simple Bash scripts.  

**Learning Outcomes (LLOs):**  
- **LLO1:** Use loops and functions to modularize scripts.  
- **LLO2:** Apply debugging techniques (e.g., `set -x`).  

**Materials Required:**  
- Linux environment (e.g., Ubuntu VM or terminal access for each student).  
- Text editor (e.g., `nano`, `vim`) for scripting.  
- Projector/screen for teacher demonstrations.  
- Sample script files (pre-prepared by teacher for distribution if needed).  
- Internet access for web scraping (assume classroom Wi-Fi or tethered connection).  

**Overall Structure:**  
- **Lecture (90 minutes):** Interactive session with explanations, live demonstrations, and step-by-step coding examples. Encourage student questions and brief hands-on trials.  
- **Small Project (60 minutes):** Guided classroom activity where students build a script incorporating the topics. Teacher facilitates by circulating, answering queries, and providing hints.  

## Part 1: Lecture (90 minutes)

### Introduction (5 minutes)
- Welcome and overview: Recap Bash Scripting I basics (e.g., if-else, variables). Introduce today's focus on advanced scripting for efficiency and modularity.  
- State LLOs and how they tie into real-world scripting (e.g., automation tasks).  
- Quick poll: Ask students about their experience with loops or functions in other languages to gauge prior knowledge.  

### Topic 1: Loops (for, while) (25 minutes - Maps to LLO1)
- **Explanation (10 minutes):**  
  - Discuss purpose: Repeating tasks efficiently.  
  - For loop: Iterates over lists, files, or ranges (e.g., processing multiple files).  
  - While loop: Runs based on conditions (e.g., reading input until end).  
  - Best practices: Avoid infinite loops; use `break`/`continue` for control.  

- **Step-by-Step Coding Guide (15 minutes - Live demo with student follow-along):**  
  1. Open a terminal and create a new script: `nano loop_example.sh`.  
  2. Add shebang: `#!/bin/bash`.  
  3. For loop example (file listing):  
     ```bash
     for file in *.txt; do
         echo "Processing $file"
         wc -l "$file"  # Count lines in each .txt file
     done
     ```  
     *Explain:* `$file` iterates over matching files; use quotes for safety.  
  4. While loop example (user input):  
     ```bash
     count=1
     while [ $count -le 5 ]; do
         echo "Iteration $count"
         ((count++))
     done
     ```  
     *Explain:* Condition checked each time; arithmetic with `(( ))`.  
  5. Run the script: `chmod +x loop_example.sh && ./loop_example.sh`.  
  6. Encourage students to modify (e.g., add `break` if count > 3) and test.  

- **Q&A:** 2-3 minutes for clarifications.  

### Topic 2: Functions and Parameter Passing (25 minutes - Maps to LLO1)
- **Explanation (10 minutes):**  
  - Purpose: Modularize code for reuse, readability (like functions in programming).  
  - Defining functions: Use `function_name() { commands; }`.  
  - Parameter passing: Access via `$1`, `$2`, etc.; `$@` for all args.  
  - Return values: Use `return` for status or `echo` for output.  
  - Scope: Local vs. global variables.  

- **Step-by-Step Coding Guide (15 minutes - Live demo):**  
  1. Create script: `nano function_example.sh`.  
  2. Add shebang: `#!/bin/bash`.  
  3. Define a function:  
     ```bash
     greet() {
         local name=$1  # Local variable for first parameter
         echo "Hello, $name!"
     }
     ```  
     *Explain:* `local` prevents global pollution.  
  4. Call with parameters:  
     ```bash
     greet "Alice"
     greet "Bob"
     ```  
  5. Advanced: Function with multiple params and return:  
     ```bash
     add() {
         sum=$(( $1 + $2 ))
         echo $sum  # Output result
     }
     result=$(add 5 3)
     echo "Sum is $result"
     ```  
     *Explain:* Capture output with `$( )`; arithmetic in `(())`.  
  6. Run and test: `chmod +x function_example.sh && ./function_example.sh`.  
  7. Students try: Pass 3 params and loop over them inside the function.  

- **Q&A:** 2-3 minutes.  

### Topic 3: Command Substitution and Array Handling (20 minutes - Maps to LLO1)
- **Explanation (8 minutes):**  
  - Command substitution: `$(command)` or `` `command` `` to capture output.  
  - Arrays: Declare with `array=(item1 item2)`; access `${array[0]}`; length `${#array[@]}`.  
  - Use cases: Storing dynamic data, processing command outputs.  

- **Step-by-Step Coding Guide (12 minutes - Live demo):**  
  1. Create script: `nano array_sub_example.sh`.  
  2. Add shebang: `#!/bin/bash`.  
  3. Command substitution:  
     ```bash
     current_date=$(date +%Y-%m-%d)
     echo "Today's date: $current_date"
     ```  
     *Explain:* Runs `date` and stores result.  
  4. Array handling:  
     ```bash
     files=($(ls *.txt))  # Array from command substitution
     for file in "${files[@]}"; do
         echo "File: $file"
     done
     ```  
     *Explain:* Quotes preserve spaces; loop over array.  
  5. Run: `chmod +x array_sub_example.sh && ./array_sub_example.sh`.  
  6. Students modify: Create array of numbers, sum them using a loop.  

- **Q&A:** 2 minutes.  

### Topic 4: Debugging Techniques (10 minutes - Maps to LLO2)
- **Explanation (4 minutes):**  
  - Common issues: Syntax errors, logic bugs.  
  - Techniques: `echo` for prints; `set -x` for trace mode; `set -e` for exit on error.  
  - Tools: `bash -n script.sh` (syntax check); `bash -x script.sh` (debug run).  

- **Step-by-Step Coding Guide (6 minutes - Live demo):**  
  1. Use previous script (e.g., `loop_example.sh`).  
  2. Add debug: Insert `set -x` at top, run `./loop_example.sh` to see trace.  
  3. Fix a bug: Introduce error (e.g., missing quote), use `bash -n` to check.  
  4. Students apply: Add `set -x` to their own test script and observe.  

- **Q&A:** 1 minute.  

### Lecture Wrap-Up (5 minutes)
- Summarize key points and LLOs.  
- Transition to project: Explain how topics integrate.  

## Part 2: Small Project (60 minutes)

**Project Title:** Build a Modular News Scraper Script  

**Objective:** Create a Bash script that scrapes news from a simple website (e.g., a public news RSS feed or homepage), searches for specific keywords, and generates a summary of matches. Incorporate loops for iterating over keywords or results, functions for fetching and parsing, command substitution for capturing outputs, arrays for storing results, and debugging techniques. (Aligns with LLO1 and LLO2.)  

*Note:* This project uses basic web scraping with tools like `curl` and `grep`; emphasize ethical scraping (e.g., respect robots.txt, rate limits) and use a scrapable site like BBC News or a public API endpoint for simplicity.

**Instructions for Students (10 minutes - Teacher explains and distributes template if needed):**
- **Script requirements:**  
  - Function 1: `fetch_page` (takes URL as param, uses `curl` to fetch content and echo it).  
  - Function 2: `search_keyword` (takes content and keyword as params, uses `grep` to find matches, stores lines in an array, and echoes count/matches).  
  - Main loop: Use `for` to iterate over an array of keywords (e.g., provided as script arguments or hardcoded).  
  - Command substitution: Capture fetched content with `$(curl -s URL)`.  
  - Output: For each keyword, echo summary (e.g., "Keyword 'X': Found Y matches" followed by snippets). Save results to a file.  
  - Debugging: Add `set -x` in functions for tracing; handle errors (e.g., if curl fails).  

- **Setup:** Teacher suggests a target URL (e.g., a simple RSS like `https://rss.nytimes.com/services/xml/rss/nyt/HomePage.xml` – parse with `xmllint` if installed, or use HTML). Provide sample keywords (e.g., "economy", "technology"). Install `curl` if needed (`sudo apt install curl`).  

- **Hints:** Use `grep -i -o -E` for keyword matching; arrays to store match lines; loop over `${keywords[@]}`.  

**Facilitated Work Time (40 minutes):**
- Students work individually or in pairs.  
- Teacher circulates: Offer guidance (e.g., "Use command substitution to store curl output in a variable"), debug common errors live (e.g., network issues, parsing bugs), encourage testing with `bash -x`.  
- Milestones: Check progress at 20 minutes (e.g., fetch function working?).  

**Debrief and Sharing (10 minutes):**
- Volunteers share their scripts (project on screen).  
- Discuss challenges (e.g., handling HTML noise in grep) and solutions.  
- Tie back to LLOs: How did loops/functions modularize the scraping process? How did debugging help with curl or grep issues?  

**Assessment:** Informal - Teacher observes participation; optional: Collect scripts for feedback.  

**Extensions (if time):** Add multiple URLs or error checking for invalid keywords.  
