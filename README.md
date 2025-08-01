- Testing Rejex web app: [Link](https://regex101.com/) 
	- Regular Expression: This is where you'll enter your RegEx.
	- Test String: This is where you'll enter the strings that your RegEx needs to match.
	- Global: This flag will be used to find all possible matches, not stopping at the first match.
	- Multi line: This flag will be used when attempting to match more than one line of text.
	- Insensitive: This flag is used to ignore the case (upper or lower) of letters.
	- Explanation: Review this panel for a detailed definition of what your RegEx characters will match.
	- Match Information: For our purposes, we will use the **global**, **multi-line**, and **insensitive** flags.
**RegEx Resources**
- http://www.regular-expressions.info/
- https://www.cheatography.com/davechild/cheat-sheets/regular-expressions/
**Web Tools**
1. https://regexr.com/
2. https://ihateregex.io/
**GUI Tools**
- RegEx Buddy
- Windows VM: [https://developer.microsoft.com/en-us/windows/downloads/virtual-machines/](https://developer.microsoft.com/en-us/windows/downloads/virtual-machines/)
**Visual Studio ADD-ONS**
- vscode RegEx
- RegexWorkBench
- RegexPreview

**Reg Examples and Explanation**
- `^.*\.txt$`
	- `.` match first char in being of line.
	- `^.*` match all chars in line
***
### Review of Log Processing

**Overview**
- Each **Log Source** is assigned a **Log Source Type**, which is associated to a **Log Processing Policy (MPE Policy)**.
- The **Log Processing Policy** contains **Log Processing Rules (MPE Rules)** which are applied to all Log Messages received from the associated Log Source.
- Hosts can have one or more Log Sources.
- Log Source Types are used to group logs that come from common hardware types, have the same data format, and have the same processing rules applied.

**Data Processor Functions**
- Log Processing Rules, also known as MPE Rules, are applied to each log message to parse data.
- Logs flow from the Log Sources to the System Monitors which then forwards the logs to the Data Processor (DP) And Logs are processed by the Message Processing Engine (MPE), contained on the DP.
- LogRhythm utilizes multiple log processing threads that can execute at the same time, making the MPE capable of processing thousands of messages per second.
- When the MPE service is started:
	- **MPE Policy** = A set of **MPE Rules** for a specific **log source type**. It is automatically loaded for each active log source and determines which rules will be applied to parse and process incoming logs.
	- **MPE Rule** = A parsing logic used to extract specific data from logs. These rules are automatically loaded as part of the MPE Policies assigned to active log sources.

<img width="1333" height="598" alt="image" src="https://github.com/user-attachments/assets/f46b8c18-106d-4338-b7b7-127a4f70563f" />

**Metadata Processing**
- During processing, LogRhythm parses, calculates, and derives metadata from log messages.
- Help for LogRhythm search tools
- **MPE Rules** are records associated to specific Log Source Types with a common event, base-rule regular expression, sub-rules, and other processing and policy settings used for processing logs.
- **MPE Rule Builder**: a tool available in the LogRhythm Client Console, is used to view, create, and edit new MPE base-rules and sub-rules.
- Performance Issue
	- The number of messages a deployment can process per second is measured by the MPS.
	- The speed at which a log is processed can greatly affect the performance of LogRhythm.
	- This is important to remember when you begin building custom RegEx rules.
- **Unidentified logs** are those logs sent through the Message Processing Engine (MPE) that were not identified when evaluated by the MPE Rules. Unidentified logs can typically be expected after adding a new Log Source.
- Unidentified logs will be displayed in the following locations:
	- Deployment Monitor 
	- Log Volume Reports 
	- Logs that have no Classification assigned in an investigation or tail.
- **LogRhythm uses a .NET RegEx Engine.** 
***
### Exercises Part 1

- `^`: start of the string, or for a multi-line string
- `$`: end of the string, or for a multi-line string
- `\d`: Numbers from 0 to 9 e.g. \d\d\d = 101
- `\D`: Char not Number e.g. \D\D\D = reg
- `\w`: Any word character a-z, A-Z, 0-9 e.g. \w\w\w = ab2 but not &@#
- `\W`: Any non-word character e.g. \W\W\W matches $#! but not abc
- `\s`: Matches any whitespace character e.g. \s\s\s matches (three spaces) but not abc
- `\S`: Matches any non-whitespace character e.g. \S\S\S matches a1_ but not (three spaces)
- `.`: Matches any character except line breaks
- `[]`: Matches any one character between the brackets
- `[^ ]`: Matches any character except, [^abc] matches anything except a, b, or c
- `[A-D]`: Matches any one character contained in the range of A through D, [A-Z0-9] matches any one alphanumeric character
- `{n}`: Matches n of the previous item e.g. \w{4} matches AAAA but not A
- `{n, }`: Matches n or more of the previous item \w{4,} matches AAAAAA but not A
- `{n,m}`: Matches at least n and at most m of the previous item if n is 0 that makes the character optional ({,9}), e.g. A{2,3} matches AA and AAA but not A or AAAA
- `?`: Matches the previous item 0 or 1 times A? matches A or nothing but not AA e.g. `Flavou?r` = Will match `Flavour` or `Flavor`
- `+`: Matches the previous item 1 or more times e.g. A+ matches A, AA, AAA but not nothing
- `*`:  Matches the previous item 0 or more times
- `(Char Group)`: Search in words in bracket
- `|`: Logical OR operator e.g. `(logrhythm\\)?\w+` = `logrhythm\jamesh` And `(this|that) book` = `this book` or `that book`
**Example**
`RegEx: User (\w+\\)?\w+ logged in`
Input Strings: 
- User Pete logged in
- User Securityco\Pete logged in
- User Securityco\Jim logged in

`\$22 and 22$`
- `\$22` Matches the string "$22"
- `22$` Matches "22" at the end of line

`^c:\\finance** would match **c:\finance`
***
### Exercises Part 2

**Example 1:**
Input String:**  
- Robert, Janet and Joseph went to the grocery store.
**Task**
- Create a regular expression that matches the whole string, but only match the comma "," and the word "and" using literal characters.
**Answer:** 
- `\w+,\s\w+\s+and\s\w+\s\w+\s+\w+\s\w+\s\w+\s+\w+\S`
- `\D+,\D+and\D+`
- `^.*?,.*and.*`
- `\w+?,\s\w+\sand.+`
- `\D+,\s\D+\sand\s\D+`
- `^.*,\s\w+\sand.*`
- `.*,\s\w+\sand\s\w+\s\w+\s\w+\s\w+\s\w+\s\D+`
- `^.*,.*and.*\.$`
- `(?:^.*?),(?:.*?)and(?:.*?)$` - this is using **capture groups**, much more advanced
- `(^.*?),(.*?)and(.*?)$` - this is using **capture groups**, much more advanced

**Example 2:** 
**Input String:**     
- John Q Public, Denver, Colorado, 80213
**Task**
- **Create one regular expression that:**
	- **Matches the whole string but only uses literal characters for the commas**
	- **Does not match names that have numbers**
	- **Does not match zip codes with letters**
- **Make sure you do not get a match if you modify the name in your Test String to contain a number.**
- **Make sure you do not get a match if you modify the zip code in your Test String to contain a letter.**
- `\D matches any non-digit, \d matches any digit`
**Answer:**
- `^\D+\s\D\s\D+,\s\d+$`
- `\D*\s\D\s\D*,\s\D*,\s\D*,\s\d+$`
- `^\D+,\s\d{5}`
- `\D*,\D*,\D*\s\d*$`
- `\D*,\D*,\D*\s\d{5}`
- `^[\D\s]+,\s[\D\s]+,\s[\d\d]+$`
- Use **\D** to match letters (with no numbers), **\s** to match spaces, and **\d** to match numeric digits.

**Example 3:** 
**Input String:** 
- `Apple is the word of the day`
- `Banana is the word of the day`
- `is the word of the day`
**Task:**
- **Create one regular expression that matches all three strings but only uses literal characters for the word “is” and the word “day”. Do not use literals for any other characters in the string.**
- In Reg101 web app: Modify the RegEx options, otherwise your string will not be matched, Click the flag icon on the right-side of the regular expression box, Select the multi-line and insensitive RegEx flags.
**Answers**
- `^.*(is)?(day)?`
- `^.*is.*day$`
- `^(\w+\s)?is\D+day$`
- The **^** will ensure the RegEx starts matching at the beginning of new line.
- As long as there is a full match of all 3 strings and the use of literal characters as specified above, the answer is correct.

**Example 4:** 
**Input String:** 
- Today we’re learning regular expressions.
**Task:**
- **Create a character set that will match only the characters used within the string.**
- **Be sure to use the escape character to ensure “.” is recognized as a literal, instead of it matching any character.**
**Answers**
- `[Today w’relninguxps\.]+`
- `^[\.Tadegilnoprsuwxy\s’]+`
- Use only **one of each type of character** in square brackets.
- As long as there is a full match and the use of a character set as specified above, the answer is correct.

**Example 5:** 
**Input String:** 
- 303-555-1212 
- 303-123-LOGS 
- 800-911-HELP
**Task:**
- **Matches all three formats of the telephone numbers.**
- **Accounts for the fact that the last 4 digits can be either numbers or letters and that telephone numbers do not use the letters Q or Z**
**Answers**
- `^\(?\d\d\d\)?[-\s]\d\d\d-[1234567890ABCDEFGHIJKLMNOPRSTUVWXY]+$`
- `^\(?\d{3}\)?.\d{3}.([ABCDEFGHIJKLMNOPRSTUVWXY|0-9]{4})$`
- Ignore “**(**” and “**)**”, allow for spaces or hyphens, and create a character set that matches both letters and numbers, but omitting Q and Z.
- As long as there is a full match for each line, the answer is correct.
**Challenge**
- Change one of the letters in a string to a Q or Z. Does it still match? It shouldn’t.
***
**Challenges**

Challenge 1: **Advanced Regular Expressions**
**Input String: OR conditions and Character Groups**
- Employee ID 0551 entered the building
- Employee ID 0021 exited the building
- Employee ID 0032 granted access to the building
**Task:**
- **Use a character group that matches any of the types of building access noted in the strings.**
- **Use an OR condition in a character group. (this|that|the other)**
**Answer:**
- `(Employee)\s\d{1,6}\s(entered|exited|granted)\D.*`
- `Employee ID .*(entered|exited|granted access to) the building$`
- `Employee\sID\s\d{4} (entered|exited|granted\saccess\sto)\sthe\sbuilding`
- `^Employee\sID\s\d{4}\s(entered|exited|granted access to).*`
- `.* (entered|exited|granted access).*building`

**Challenge 2: Character Set and Character Groups**
**Input String:**
- Session ID [0xa487c11-1111] started
- Session ID [0x33a0a56-75] ended
- Session ID [0x77df0011-322] terminated
**Task:**
- **A character set [ ] with the “^” not operation that matches the Session ID value.**
- **Character groups ( ) to match “started”, “ended” and “terminated” as literal characters.**
**Answer:**
- `(Session ID)\s\[?(0x)\w{5,15}(-)\d{1,6}\]?\s(started|ended|terminated)`
- `^Session ID \[[^\]]+\] (started|ended|terminated)$`
- `Session ID .*[^9g-w] (started|ended|terminated|)`
- `Session\sID\s\[0x[^g-z]+\s(started|ended|terminated)`

**Challenge 3: Repetition Characters and Character Groups**

**Input String:**     
- (719) 555-1212 
- 1-719-555-1212 
- +1 719 555-1212 
- +1 (719) 555-1212 x234
**Task**
- **Matches all the telephone number strings.** 
- **Uses repetition characters in character groups to match all digits.** 
- **Accounts for the fact that the area code and first three digits have to be numbers, the last 4 digits can be either numbers or letters, and that telephone numbers don’t use the letters Q or Z.**
**Answer**
- `^(\+?\d(\s|-))?\(?\d\d\d(\)?\s|-)\d\d\d-\d\d\d\d( x\d+)?$`
- `^\+?(\d+)?[-\s]?\(?(\d+)\)?[-\s]+(\d+)[-\s]([^QZ\s]+)\s?(x\d+)?`

**Challenge 4: Character Groups**
**Input String:**
- Package 123456789 has arrived
- Package 12345678901 (special order) has arrived
- Package 123456789012 has arrived
**Task**
- **Matches all of the strings.** 
- **Matches all of the text that is identical in each string with literal characters.** 
- **Uses repetition characters to match only package numbers between 9 and 12 characters in length.**
**Answer**
- `^Package ([\d]{9,12} (\(special order\) )?)?has arrived$`
- Challenge: Add a few more digits to the last package number string. Does your RegEx find a match? It should not.

**Challenge 5: Repetition Characters and Character GroupsRepetition Characters**
**Input String:**     
- 2/16/2009 2:13 AM 
- 06 22 2008 17:22:28 10.6.93.99
**Task:**
- Create **two** separate regular expressions; one to match each string. 
- Match the date, time, and IP Address field as applicable. 
- For the date and time fields, use repetition characters to allow for 1 – 2 characters. 
- For the IP Address, use repetition characters to allow each octet to range from 1 – 3 characters.
**Answer:**
- 1st: `[0-9]{1,2}\/[0-9]{2}\/[0-9]{4}\s[0-9]{1,2}:[0-9]{1,2}\s(AM|PM)`
- 1st: `^\d{1,2}\/\d{1,2}\/\d{4} \d{1,2}:\d{1,2} AM$`
- 2nd: `^\d{2} \d{2} \d{4} \d{2}:\d{2}:\d{2} \d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}$`
***
## RegEx in LogRhythm

- **Regex by default stops at the first match** unless the pattern is designed to find more.
- **Quantifiers** (like `*`, `+`, `?`, `{}`) control **how many times** a character/pattern must appear.
- Two types of quantifiers:
    1. **Greedy** (`*`, `+`): Match **as much as possible**.
    2. **Non-greedy** (`*?`, `+?`): Match **as little as needed**.
- Non-greedy quantifiers are useful especially when using the dot `.` (which matches anything).
- Use `.*?` or `.+?` instead of `.*` or `.+` when you want **precise, minimal matches**.

|**Function**|**Greedy Characters**|**Non-Greedy Characters**|
|---|---|---|
|Match the previous item 0 or more times|`*`|`*?`|
|Match the previous item 1 or more times|`+`|`+?`|
|Match the previous item 0 or 1 times|`?`|`??`|
|Match exactly n times|`{n}`|`{n}?`|
|Match at least n times|`{n,}`|`{n,}?`|
|Match from n to m times|`{n,m}`|`{n,m}?`|

**Capture Groups**
- **Capture Groups:** RegEx will automatically capture that information in memory for reuse.
- Capture groups can also be assigned names. These are called **named capture groups**.
	- you can access the data matched by the RegEx by its given capture group name.
	- Format: `(?<name>regex).` e.g. `(?<name>this|that) book`
- The following RegEx uses a named capture group to extract metadata from a log message:
	- IP:`(?<ip>\d{1,3}(?:\.\d{1,3}){3})
	- **IP:** indicates a literal match of characters in the string.
	- The **? character** at the beginning of the group says to name the group as specified by the information contained inside the angle brackets
	- `?:`: to make IP format not changeable and Structuring the pattern without capturing 
### Parse Metadata into Fields Using Tags
- The named capture groups used in LogRhythm are the metadata fields.

**Overloading Tags in LogRhythm**
- **Overloading a tag:** Instead of using the default RegEx LogRhythm has assigned for a tag, if necessary, you can specify different RegEx characters to use for that tag.
- An overloaded tag uses the standard RegEx format for Named Capture Groups: `(?<name>regex)`.
- The ? at the beginning of an overloaded tag, in LogRhythm, says to replace the default RegEx for the `<tag>`, and use these specified RegEx characters instead, to match the text in the string.
- **Overloading Examples:** 
	- By default in LogRhythm, the `<login>` tag uses the value `\w+.`.
	- this "User john.doe opened AnnualReport.pdf" mapped to `User (?<login>\w*\.?\w*) opened (?<object>\w*\.pdf)`
- To replace the default RegEx, the following syntax should be used: `(?<tag_name>regex)`.
- It is important to note that some tags should not be overloaded such as IP Address (Impacted) and NAT IP Address (Impacted) but there are others, such as Source IP and Source NAT IP Address.
- Do not  overload these: `<sip>, <dip>, <snatip>, <dnatip>` They use explicit patterns to match IP addresses.
- The **.*?** characters will match any character, including the **\,** **@,** and **periods**, an unlimited number of times.
- This `User\s(?<User>.*?)\s` mean get any thing after user word and stop when you find first space and just make this once this because we add `?` to make it search one time, and this will match mail, user as mmd\Mohamed, user as Mohamed
**Resources**
- [LogRhythm Community](https://mycommunity.exabeam.com/kb/training-reference/custom-mpe-rules-using-regular-expression-training-materials/40465)
- [LogRhythm Doc](https://docs.logrhythm.com/lrsiem/docs/download-pdfs)
***
## Creating Custom MPE Rules

- **Location:** Client Console > Tools  > Knowledge > MPE Rule Builder
**General Settings**
- **Enter a Rule Name** 
	- Use the **VMID** in the rule name, When the matching log message contains a vendor message ID such as an event ID in Windows Event Logs, it is good to include the ID in the name of the rule.
	- Use the **service** in the rule name.
	- All rule names should contain a **brief description**
	- Ex: **EVID 528 : Failed Authentication : Bad Username or Password**.
- **And select a Common Event for your new custom MPE Rule.**
	- MPE Rules are assigned a Classification and Common Event that describes the activity contained in the associated log messages.
	- There is 29,000 Common Events.
	- **Do not create new Common Events. LogRhythm recommends using one that exists in the list.**
**Rule Status**
- **Production**
	- This allows the rule to be applied to log messages by the MPE in LogRhythm. 
	- The logs that match the MPE Rule can be displayed in the Personal Dashboard
	- Custom MPE Rules can be edited, even in Production mode. However, System MPE Rules (created by LogRhythm) cannot be edited.
- **Test**
	- The rule can be applied by MPE **for testing**, but:
		- Matching logs **won’t appear** in the Personal Dashboard.
		- N**o alarms** will be triggered
	- The rule will be **automatically disabled** if:
		- It matches **1000 logs**, or MPE has **performance issues**
	- Check `scmedsvr.log` for performance-related messages.
- **Development**
	- While creating new rules, they should be set to Development status.

<img width="1459" height="711" alt="image" src="https://github.com/user-attachments/assets/09b2e386-87f1-426c-9927-6296f5f62362" />

**Processing Settings**
- The MPE uses the Processing Settings to aid in the interpretation of the RegEx rule.
- The first few lines in the _Processing Settings_ tab contain checkboxes to enable additional options for the RegEx rules.
- Config 
	- Check the box in Ignore Case
	- **Host Context:** 
		- Set to **Tags Normal** if `<sip>, <sname>,<dip>,<dname>,<sipn>, or <dipn>` exist in the RegEx.
		- Set to **Local** if these tags don’t exist in the RegEx.
		- Should never be set to Tags Reverses in the Base-rule and rarely used in Sub-rules.
	- **Origin Host Determine via:**
		- Set to **Tags** if RegEx contains `<sip>, <sname>, or <sipn>`.
		- Set to **N/A** if these tags don’t exist.
		- Should **rarely** be set to **Log Message Source**.
	- **Impacted Host Determine via:**
		- Set to **Tags** if RegEx contains `<sip>, <sname>, or <sipn>`.
		- Set to **Log Message Source** if these tags don’t exist.
		- Should **rarely** be set to **N/A**.
	- **Impacted Application Determine via:**
		- Set to **Tags Normal** if the RegEx has tags for dport, with protnum or protname.
		- Set to **Tags Reversed** if the RegEx has tags for sport, with protnum or protname.
		- Else set to **N/A**.
		- In certain circumstances, if you know the application, set to **Direct Assignment**.
	- **Assigned Application:**
		- If Impacted Application is set to **Direct Assignment**, set this to the appropriate Application Name.

**Default Policy Settings**
- **Policy Settings** help MPE manage data.
- Options in the **Default Policy Settings** tab (inside Rule Builder) are **not selected by default**.
- Even if you select them here, they’re usually **overwritten** by settings in the **Log Processing Policies** tab in **Deployment Manager**.
- Best practice: Configure them using the **MPE Policy Rule Editor** instead.
***
#### Log Message Source Type Associations
- LogRhythm has **predefined Log Source Types** for hundreds of devices and apps.
- Log Source Type identifies **where the log came from** (device or application).    
- When creating **custom MPE Rules**, you **must associate** a Log Source Type.
- Choosing the **correct Log Source Type** is critical for proper rule function.
- Use the scroll bar to review source types, which are organized by **collection method and device type** (e.g., Syslog, API, Windows Event, NetFlow, etc.).
- Select the matching type by checking the box.
- For custom or new devices, you may need to **create a new custom Log Source Type** before creating rules.
- **Remember: Log Source Types are updated via the Knowledge Base.**

**Base-rule Regular Expressions**
- In the MPE Rule Builder, the _Base-rule Regular Expression_ tab is used to enter the RegEx for your new custom MPE Rule.
- Base-rules contain a tagged regular expression used to identify the pattern of a log and isolate interesting pieces of metadata.
- Using tags, these data strings can be directed to metadata fields.
- **Scratch Pad**
	- The **Scratch Pad** tab is a workspace for building and testing parts of your RegEx.
	- Use it to **copy, paste, and edit** RegEx snippets before adding them to the final **Base-rule Regular Expression** tab.
	- Helpful for experimenting or reusing parts from other patterns.

**Catch-all Rules**
- **Catch-all rules** are part of the **Base-rule** and used to **parse logs that don’t match other rules** for a specific Log Source Type.
- They extract **common metadata** (like log type or severity: Info / Warning / Error).
- Useful for:
    - Logs with **unknown format**
    - Logs with **new vendor message IDs**
- Catch-all rules act as a **fallback** to ensure no log goes unprocessed.
- You should:
    - **Monitor** catch-all hits
    - **Create new specific rules** when new patterns are identified
- **Naming format:**  
    `Catch-All: NAME_HERE` (follow this format for consistency)
- Older **Catch-all rules by LogRhythm** may include **Levels 1–4** in their names.
