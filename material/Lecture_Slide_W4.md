---
marp: true
theme: default
paginate: true
headingDivider: 1
---
# Bash Shell Scripting II: Advanced Technique
**Linux Operating System Course**
**Instructor: Pharino Chum**  
**Duration: 2.5 Hours**  
**Date: December 15, 2025**

# Agenda
**Today's Session**  
1. Introduction & Recap (5 min)  
2. Loops: for & while (25 min)  
3. Functions & Parameter Passing (25 min)  
4. Command Substitution & Arrays (20 min)  
5. Debugging Techniques (10 min)  
6. Lecture Wrap-Up (5 min)  
7. Hands-On Project: News Scraper Script (60 min)


# Learning Outcomes
**By the end of this session, you will be able to:**  
- **LLO1:** Use loops and functions to modularize scripts  
- **LLO2:** Apply debugging techniques (e.g., `set -x`)  

**Real-world relevance:** Automation, system administration, data processing


# Topic 1 – Loops (for, while)
**Why Loops?**  
- Repeat tasks efficiently without duplicating code  

**For Loop**  
```bash
for file in *.txt; do 
    echo "Processing $file"
    wc -l "$file"   # Count how many line in each file
done
```
#
**While Loop**  
```bash
count=1
while [ $count -le 5 ]; do
    echo "Iteration $count"
    ((count++))
done
```

**Best Practices**  
- Use quotes around variables  
- Avoid infinite loops (`break`, `continue`)


# Topic 2 – Functions & Parameter Passing
**Why Functions?**  
- Modular, reusable, readable code  

**Defining a Function**  
```bash
greet() {
    local NAME=$1
    echo "Hello, $NAME!"
}
```
#
**Calling Functions**  
```bash
greet "Alice"
greet "Bob"
```

**Returning Values**  
```bash
add() {
    echo $(( $1 + $2 ))
}
result=$(add 5 3)
echo "Sum is $result"
```

**Tips**  
- Use `local` for variables  
- `$1`, `$2`, … for parameters; `$@` for all


# Topic 3 – Command Substitution & Arrays
**Command Substitution**  
```bash
current_date=$(date +%Y-%m-%d)
echo "Today's date: $current_date"
```

**Arrays**  
```bash
files=($(ls *.txt))          # Populate from command
echo "First file: ${files[0]}"
echo "All files: ${files[@]}"
```

**Looping Over Arrays**  
```bash
for file in "${files[@]}"; do
    echo "File: $file"
done
```

**Key Points**  
- Use `"${array[@]}"` to preserve spaces  
- `${#array[@]}` → array length


# Topic 4 – Debugging Techniques
**Common Problems**  
- Syntax errors  
- Logic bugs  
- Unexpected input/output  

**Debugging Tools**  
- `set -x` → Trace execution  
- `set -e` → Exit on error  
- `echo` statements for manual tracing  
- `bash -n script.sh` → Syntax check (no execution)  
- `bash -x script.sh` → Run with tracing  

**Example**  
Add at top of script:  
```bash
set -x
```


# Lecture Summary
**Key Takeaways**  
- Loops: Automate repetition  
- Functions: Modularize and reuse code  
- Command substitution & arrays: Handle dynamic data  
- Debugging: Find and fix issues quickly (`set -x`)  

**Next:** Hands-on Project!


# Hands-On Project (60 min)
**Project Title:** Build a Modular News Scraper Script  

**Objective**  
Create a Bash script that:  
- Fetches a news page/RSS feed using `curl`  
- Searches for specific keywords
- Outputs a summary of matches  

**Required Elements**  
- Function: `fetch_page` (uses `curl`)  
- Function: `search_keyword` (uses `grep`, stores matches in array)  
- Loop over array of keywords  
- Command substitution to capture content  
- Save results to a file  
- Use `set -x` for debugging  


# Project Details
**Suggested Tools & Commands**  
- `curl -s URL` → Silent fetch  
- `grep -i "keyword"` → Case-insensitive search  
- Arrays to store matching lines  
- Output example:  
  `Keyword 'technology': Found 5 matches`

**Sample Target**  
- RSS: `https://rss.nytimes.com/services/xml/rss/nyt/HomePage.xml`  
- Or any simple news homepage (check robots.txt!)

**Ethical Note**  
- Respect rate limits and robots.txt  
- Use only for learning purposes


# Project Workflow
1. Define functions  
2. Create keyword array (hardcode or args)  
3. Fetch content with command substitution  
4. Loop through keywords → call search function  
5. Save/output results  
6. Test with `bash -x script.sh`  

# Teacher Support
- I will circulate to help  
- Share your script at the end!

# Suggested Self-Study Topics
- Practice more advanced scripting  
- Explore `sed`, `awk`, APIs in Bash  

# <!--fit-->**Thank you! Questions?**