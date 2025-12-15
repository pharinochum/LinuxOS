---
marp: false
theme: default
paginate: true
headingDivider: 1
---

# Lab Manual: Hands-On Project - Build a Modular News Scraper Script

**Course:** Linux Operating System  
**Topic:** Bash Shell Scripting II: Advanced  
**Project Duration:** 60 minutes  
**Date:** December 15, 2025  

---

## Project Overview

In this lab, you will create a Bash script that performs basic web scraping to fetch news content and search for specific keywords. The script will demonstrate the advanced Bash concepts covered in today's lecture: loops, functions, command substitution, arrays, and debugging techniques.

**Learning Outcomes**  
By completing this project, you will:  
- Use loops and functions to modularize your script (LLO1)  
- Apply debugging techniques such as `set -x` (LLO2)  

**Important Note on Ethics**  
- This is for educational purposes only.  
- Always respect a website's `robots.txt` file and terms of service.  
- Do not overload servers with rapid requests.  
- We will use publicly available, scrape-friendly sources (e.g., RSS feeds or simple news pages).

---

## Prerequisites & Setup

1. Ensure you have internet access.  
2. Required tools (usually pre-installed on Ubuntu/Linux):  
   - `bash`  
   - `curl` (for fetching web content)  
   - `grep`  
   If `curl` is missing, install it:  
   ```bash
   sudo apt update && sudo apt install curl -y
   ```

3. Choose a target source (suggested by instructor):  
   - Example RSS feed: `https://rss.nytimes.com/services/xml/rss/nyt/HomePage.xml`  
   - Alternative simple HTML page (if RSS parsing is complex): a lightweight news site homepage.  
   **Instructor will confirm the URL to use in class.**

---

## Project Requirements

Your script (`news_scraper.sh`) must include the following features:

1. **Functions**  
   - `fetch_page`: Takes a URL as parameter, fetches content using `curl`, and returns/echoes the content.  
   - `search_keyword`: Takes fetched content and a keyword as parameters, searches for the keyword (case-insensitive), stores matching lines in an array, and outputs the number of matches and optionally the matches themselves.

2. **Arrays**  
   - An array of keywords to search for (either hardcoded or passed as script arguments).

3. **Loops**  
   - A `for` loop to iterate over the keywords and perform searches.

4. **Command Substitution**  
   - Use `$(curl ...)` to capture the web page content into a variable.

5. **Output**  
   - For each keyword, print a summary such as:  
     ```
     Keyword 'technology': Found 7 matches
     ```  
   - Optionally list the matching lines (snippets).  
   - Save all results to a file (e.g., `results.txt`).

6. **Debugging**  
   - Include `set -x` inside functions or at the top (comment it out when done).  
   - Add basic error handling (e.g., check if `curl` succeeded).

---

## Step-by-Step Guide

### Step 1: Create the Script File
```bash
nano news_scraper.sh
```
Add the shebang line at the top:
```bash
#!/bin/bash
```

Make it executable later:
```bash
chmod +x news_scraper.sh
```

### Step 2: Define the fetch_page Function
```bash
fetch_page() {
    local url=$1
    # Use -s for silent, -L to follow redirects, -f to fail quietly
    content=$(curl -s -L -f "$url")
    if [ $? -ne 0 ]; then
        echo "Error: Failed to fetch $url" >&2
        return 1
    fi
    echo "$content"
}
```

### Step 3: Define the search_keyword Function
```bash
search_keyword() {
    local content=$1
    local keyword=$2
    
    # Find matching lines (case-insensitive)
    matches=($(echo "$content" | grep -i -o "...\{0,50\}$keyword.\{0,50\}..."))
    
    # Alternative simple count:
    # count=$(echo "$content" | grep -i -c "$keyword")
    
    echo "Keyword '$keyword': Found ${#matches[@]} matches"
    
    # Optional: print matches
    for match in "${matches[@]}"; do
        echo "  -> $match"
    done
}
```

### Step 4: Main Script Logic
```bash
# Target URL (replace with instructor-provided URL)
URL="https://rss.nytimes.com/services/xml/rss/nyt/HomePage.xml"

# Fetch content
page_content=$(fetch_page "$URL")
if [ $? -ne 0 ]; then
    echo "Exiting due to fetch error."
    exit 1
fi

# Array of keywords (you can modify these)
keywords=("technology" "economy" "science" "health")

# Output file
output_file="results.txt"
> "$output_file"  # Clear file

echo "News Scraper Results - $(date)" | tee "$output_file"
echo "Source: $URL" | tee -a "$output_file"
echo "----------------------------------------" | tee -a "$output_file"

# Loop over keywords
for kw in "${keywords[@]}"; do
    echo "" | tee -a "$output_file"
    search_keyword "$page_content" "$kw" | tee -a "$output_file"
done

echo "" | tee -a "$output_file"
echo "Scraping complete. Results saved to $output_file"
```

### Step 5: Add Debugging (Optional during development)
Add this near the top (after shebang):
```bash
set -x   # Remove or comment out when script is working
```

### Step 6: Run and Test
```bash
./news_scraper.sh
```

Use debugging mode:
```bash
bash -x news_scraper.sh
```

---

## Tips & Common Issues

- RSS feeds are XML → `grep` will still find text inside `<title>` and `<description>` tags.  
- HTML pages may have noise; keep keywords specific.  
- If content is too large, consider piping through `head` during testing.  
- Use `grep -i` for case-insensitive search.  
- To extract cleaner text from RSS, you can later learn `xmllint` or `sed`.

---

## Submission & Debrief

- Save your final script as `news_scraper.sh`.  
- Be prepared to demonstrate it or share your screen during the debrief.  
- Discuss with the class:  
  - What challenges did you face?  
  - How did functions and loops make your code more modular?  
  - How did `set -x` help you debug?

**Extensions (if you finish early)**  
- Accept keywords as command-line arguments (`$@`).  
- Support multiple URLs.  
- Count total occurrences more accurately with `grep -o`.  
- Add color output using escape codes.

**Enjoy building your news scraper!**  
Ask the instructor for help at any time.