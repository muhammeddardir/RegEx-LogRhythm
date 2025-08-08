- Testing Rejex web app: [Link](https://regex101.com/) 
	- Regular Expression: This is where you'll enter your RegEx.
	- Test String: This is where you'll enter the strings that your RegEx needs to match.
	- Global: This flag will be used to find all possible matches, not stopping at the first match.
	- Multi line: This flag will be used when attempting to match more than one line of text.
	- Insensitive: This flag is used to ignore the case (upper or lower) of letters.
	- Explanation: Review this panel for a detailed definition of what your RegEx characters will match.
	- Match Information: For our purposes, we will use the **global**, **multi-line**, and **insensitive** flags.
**RegEx Resources**
- [Link1](http://www.regular-expressions.info/)
- [Link2](https://www.cheatography.com/davechild/cheat-sheets/regular-expressions/)
**Web Tools**
1. [RegExr](https://regexr.com/)
2. [Ihateregex](https://ihateregex.io/)
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
**MPE Rule Builder – Test Center**
- The **Test Center** is essential for **creating and validating MPE Rules**.
- Use it to test whether your **RegEx correctly parses** data (e.g., VMID, VendorInfo) from log samples.
- First, **import log samples** from the target Log Source Type.
- Logs can be imported:
	- From the **LogRhythm Log Viewer** (as shown in the course)
	- Or **manually** added into the Test Center.
**Importing and Testing Logs in the Test Center**
- To add logs to the **Test Center**:
	- Use **"Import Log Messages Manually"** to paste logs directly.
	- Use **"Import LogRhythm Log Export (LLX) File"** to import logs from a `.llx` file.
- From the same menu, you can:
	- **Export** log messages.
	- **Clear** unwanted log messages.
- To test your RegEx:
	- Click **"Test Selected log samples"** or **"Test All log samples"** to check if it parses the data correctly.
**Test Center Results and Performance**
- Test results appear in a **pop-up window**.
- Compare:
    - **Total logs processed** vs. **logs matched** by your RegEx.
- Check the **average logs per second** rating:
    - Aim for **Fantastic** or **Great** for optimal MPE performance.
- If the rating is lower:
    - **Tune your RegEx** to improve speed.
    - Slow rules reduce the **Messages Per Second (MPS)** the Mediator can handle.
**General RegEx Tips**
- **Start with a reliable literal**: If a consistent text exists in all logs, begin your RegEx with it (e.g., use `EVID:<vmid>` instead of `^.*?EVID:<vmid>`).
- **Use non-greedy matching (`.*?`)** to skip over unimportant parts of the log.
- **Stop matching once key data is captured** — no need to parse the entire log line.
- **Build and test incrementally** in the Rule Builder: match small parts, test, check metadata, and continue improving.
#### Writing RegExes for Use in LogRhythm
- Suppose you've noticed the following log message showing in a report as an **Unidentified** log or a log which has **no Classification** assigned in LogRhythm.
```json
10/11/2008 08:15:00.0154 EVID:4140 – A ‘catastrophic failure’ has occurred. Your network is hosed.
```

**Steps to create MPE**
**Example 1**
1. You did some research and determine that the log contains a new error code recently created by the vendor of your firewall device.
2. You realize that because this is a brand new EVID code, it does not yet have an associated MPE Rule in LogRhythm.
3. You begin by importing this log message into the MPE Rule Builder tool.
4. To create a RegEx for use in LogRhythm, you need to begin by thinking in terms of metadata fields.
5.  You'll use the _LogRhythm MPE Rule Builder Guide_ to locate the associated metadata field display name, tag, and default RegEx characters. download this document from [LogRhythm Community](https://community.logrhythm.com/t5/Training-and-Reference/Custom-MPE-Rules-using-Regular-Expression-Training-Materials/ta-p/40465) or [LogRhythm Docs](https://docs.logrhythm.com/lrsiem/docs/download-pdfs).
6. Create Field for Vendor ID
	1. You see Vendor Message ID listed in the **Display Field**. `<vmid>` is the **Tag** which uses the **Default Regex** **\w+** to match the text for the EVID number.
	2. You note that the Vendor Message ID number appears after the text string "EVID:", and there is a space where the number ends.
	3. The base Regular Expression for the MPE Rule is: `EVID:<vmid>` we call capture group name to apply saved regex with it.
7. Error Field Parsing under venderinfo tag
	1. The log message includes special characters (e.g., periods, apostrophes, spaces) that the default `\w+` RegEx can't match. To capture the full error text, the second tag is overloaded using:
	2. `EVID:<vmid>\s(?<vendorinfo>.*?)$`
**Example 2**

```json
02 20 2021 00:00:30 1.1.1.1 <LOC0:INFO> Feb 20 00:00:51 1,2021/02/20 00:00:51,0002C100617,TRAFFIC,end,33,2021/02/20 00:00:45,1.6.5.5,1.1.1.4,0.0.0.0,0.0.0.0,Inbound_vWire,,,ping,vsys1,untrust,trust,ethernet1/1,ethernet1/2,LogRhythmForwarder,2021/02/2000:00:51,176828,1,0,0,0,0,0x0,icmp,allow,120,120,120,2,2021/02/20 00:00:37,6,any,0
```

1. Enter a **Rule Name**.
2. Walk Through Example: Palo Alto
3. Assign an appropriate **Classification** and **Common Event** `Network Traffic`.
4. Next, associate the new rule to the appropriate **Log Source Type**.
5. Import the log message into the **Test Center** then **Import Log Massage Manually** and Past The Log to Import, Check box `wrap text` and press Ok.
6. Determine the data to parse from the log and their associated LogRhythm metadata fields. We will use the following:
	1. TRAFFIC – Vendor Message ID
	2. 1.6.5.5 – IP Address (Origin)
	3. 1.1.1.4 – IP Address (Impacted)
	4. 0 – TCP/UDP Port (Origin)
	5. 0 – TCP/UDP Port (Impacted)
	6. icmp – Protocol
	7. allow – One variation of what happened with this traffic; will be used in a Sub-rule.
	    - For example, this traffic activity could have also been described as **deny** or **fail**.
	    - Sub-rules will be created with a different Common Event for each type of activity
	8.  Use the _LogRhythm MPE Rule Builder Parsing Guide_ to locate the associated tags.

|**Data to Parse**|**Associated Tag**|
|---|---|
|TRAFFIC – Vendor Message ID|`vmid`|
|1.6.5.5 – IP Address (Origin)|`sip`|
|1.1.1.4 – IP Address (Impacted)|`dip`|
|0 – TCP/UDP Port (Origin)|`sport`|
|0 – TCP/UDP Port (Impacted)|`dport`|
|icmp – Protocol|`protname`|
|allow|to be used in Sub Rule Tag1|
7. Begin entering the **Base-rule Regular Expression**.
	1. Use wildcards to skip over unnecessary information.
	2. Use literal characters to match text before and after the data to be parsed into the desired metadata field.
	3. Click to **Test Selected** log message. Verify the data was parsed correctly into the metadata field.
8.  Enter additional RegEx and tags to parse data into another metadata field. Click **Test Selected** log message. 
9. Final Reg: `^.*,<vmid>,end,.*?:45,<sip>,<dip>,`
10. Be sure to click **Save** for your new custom MPE Rule.
***
#### Exercises Part3: Create a New MPE Rule 
**Create a new custom MPE Rule that matches the vendor ID message found in logs.**

**Part One: Locate log messages containing EVID 0012**
1. Open Client Console > Investigate Tap > Config New Investigation > Select Log Source Type > Select Search button > R_click on Entity and select "Check all displayed" all then Ok > Next without specific any event > Click Next to accept "Query default log repository" > Write Name of Search > Save Then Launch > after complete select Log viewer tap >  then select new EVID  > then select all logs > R_click and select "Copy Selected Logs to Rule Builder" > Yes.

**Part Two: Use the MPE Rule Builder tool to create a new MPE Rule**
1. Go to Rule Builder 
2. Add Rule Name: EVID:0012
3. Open Common Event and Select common event (Classification = Audit Access Success) and (Common Event = Object Accessed)
4. Make Rule Status in **Development**
5. Add Source Type from widget **Log Message Source Type Association**

**Part Three: Determine tags required to parse metadata from logs**
1. From Download PDF

**Part Four: Write a new regular expression for your Custom MPE Rule**
1. Click Test Center (We can see log Massage we copied it that contain EVID:0012)
2. in Base-Rule Regular Expression write: `EVID:<vmid>\s(<dip>|(?<dname>/*?)):/*?user\s<login>\sfrom\s(<sip>|(?<sname>/*?)).*?\sObject\s=(?<object>.*?)\sSession ID:\s(?<session>.*?)$`
3. Click Save
4. Click Test All
5. Review the result in Rule Builder Test Result pop-Up windows.
	1. How many this logs matched the rules
	2. Review the RegEx Average logs/second (total) must be (fantastic)

**Part Five: Configure additional settings for your Custom MPE Rule**
**Select Processing Settings**
1. Ensure the "match multi-line logs message" is un checked 
2. Ensure "Ignore Case when evaluating log message" is checked to simplified RegEx development and handle possible change in text by device.
3. Ensure "Disable Rule Performance Monitoring" in un checked 
**Since this Rule has both Source and Destination Tags**
4. Host Context make it "tag Normal" this mean we will use default `<sip>, <sname>, <dip>, <dname>`
5. Origan Host determine via make it "Tags"
6. Impacted Host determine via make it "Tags"
7. Impacted Application determine via make it "N/A": Since this rule is not parsing destination port or protocol, and it is not immediately obvious what application is generating the log, the **Impacted Application determined via:** setting should be set to **N/A**. This is the default setting.
8. Save and close windows

#### Note saved Search we can find it in tap "Windows"
***
## Enabling Custom Rules

### Sub-rules
- **Sub-rules** are used to extract **specific data** that may **not appear in all logs**, like event IDs, messages, or usernames.
- They work **under a Base-rule** and refine log parsing when multiple logs match the same Base-rule.
- Each Sub-rule includes a **specific RegEx** for extracting targeted fields.
- A **Base-rule can have many Sub-rules** to help normalize log data more accurately.
**Sub Rule Example**
- Base-rule detects a system shutdown; Sub-rule identifies the **shutdown reason**.
**How MPE Processes Logs**:
- MPE checks the log against **Base-rules**.
- If matched → it checks associated **Sub-rules**.
- If a Sub-rule matches → the log is linked to that Sub-rule.
- If no Sub-rules match → the log stays with the Base-rule.
**Summary**
- **Base-rules = general match**  
- **Sub-rules = specific details**  
- They work together for **better log normalization**.
**Location**
- Location of Sub-rules in the MPE Rule Builder
**columns in Sub Rule**
- Name: Each Sub-rule is given a unique name
- Classification
- Common Event

**Creating Sub-rules**
- You Must have existing Base Rule
- Right-click inside the Sub-rule tab and select **New**.
- **Note:** You can also Clone existing Sub-rules if you are creating multiples with the same Classification and Common Event, but perhaps with slight variations of VMIDs.
- The **Sub-rule Properties** window opens, allowing you to configure your new Sub-rule.
**Mapping Tag**
- **Sub-rules** use **Mapping Tags** to define **criteria** that must be met in metadata fields for a log to match a specific **Common Event**.
- These criteria are set in the **Match** column and checked against values in the **Value** column.
- To match a log with a specific rule, we use something called **Mapping Tags**, which define:
	- **Which field** to compare,
	- **What value** to compare it to,
	- And **how** the comparison should be done.

| **Match Type**      | **Description**                                | **Example** |
| ------------------- | ---------------------------------------------- | ----------- |
| `Anything`          | Matches **any value** using wildcard `*`       | `*`         |
| `Nothing`           | Matches an **empty or null** value             | (no value)  |
| `Pattern`           | Matches a **Regex pattern**                    | `(this      |
| `Equal To (=)`      | Matches an **exact value**                     | `=shutdown` |
| `Not Equal To (!=)` | Matches **any value except** the specified one | `!=error`   |

**Example**
- If you have a log with a field named `Action`  and you want the Sub-rule to trigger **only when** the `Action` equals `"allow"`, you would configure it as (when action = allow): 
	- **Field:** Action
	- **Match:** Equal To
	- **Value:** allow

**The Sub-rule Tags in Mapping Tags**
- The Field names highlighted are not metadata fields in LogRhythm.
- Default RegEx = .*
- Understanding Sub-rule Tags

```Json
^.*?(?:\d){2}:(?:\d){2}:(?:\d){2} .*?<process>\[\d*\]: FTP LOGIN (?<tag1>SUCCEEDED|FAILED) FROM (<sip>|(?<sname>)(\s|$)
```

```Json
Log Messages:

10 11 2008 22:54:53 1.1.1.2 <FTPD:NOTE> Oct 12 05:54:53 ftpd[87428]: FTP LOGIN
SUCCEEDED FROM 21.1.1.1

10 11 2008 22:54:55 1.1.1.2 <SAU2:NOTE> Oct 12 05:54:55 ftpd[87428]: FTP LOGIN
FAILED FROM 21.1.1.1, apache

```

The Base-rule should have create two Sub-rules created for the `<tag1>` values captured from the logs
- One Sub-rule will match logs containing **SUCCEEDED** login attempts. added in tag1
- One Sub-rule will match logs containing **FAILED** login attempts. added in tag1
***
Example:

```Json
LOG MESSAGE:

<Event xmlns-='[http://schemas.microsoft.com/win/2004/08/events/event](https://www.google.com/search?q=http://schemas.microsoft.com/win/2004/08/events/event)'><System><Provider Name='SQLISPackage100'/><EventID Qualifiers='16385'>12289</EventID><Level>Information</Level><Task>None</Task><Keywords>Classic</Keywords><TimeCreated SystemTime='2017-06-04T06:00:00.0000000Z'/><EventRecordID>25582324</EventRecordID><Channel>Application</Channel><Computer></Computer><Security UserID=''/></System><EventData>Package "" finished successfully.</EventData></Event>
```

```json
BASE-RULE REGEX:

Name='(?<tag2>.*?(SQLSERVER|SQLISPackage100|SQLAgent|SQL\$).*?)'.*?<EventID.*?>
(?<vmid>\d+)<.*?Level(?<severity>.*?)<.*?Com.*?>(?<dname>.*?) (.*?\?)?/C((.?\r|\n)
{3}succ.*?:(?<tag3>.*?) (\r|\n)(.*?(\r|\n)){2}sess\S+:(?<session>\d+)(.*?(\r|\n)){5}objS+:
(?<objectname>\w+)(.*?(\r|\n)){3}serS+name:((?<domain>.*?)\\)?
(?<login>\S+)(.*?(\r|\n)){9}object_name:(?<object>.*?)(\r|\n)staS+:(?<command>.*?)(\r|\n)?.*?pr
.*? id of (?<processid>\d+)?
(.*?Sch.*? Job '(?<object>.*?)')?.*?Status:
(?<tag3>\w+)?(.*?for user '((?<domain>.*?)\\)?
(?<login>.*?)'(.*?database
'(?<object>.*?)')?.*?\[CLIENT:\s+(<sip>|(<)?(?<sname>.*?)>)?
\])?
(.*?UserID='((?<domain>[^/']+)\|\\)?(?<login>.*?)'(.*?Pac\w+
"(?<object>.*?)")?.*?comS+
(?<command>.*?)\.?.*?\((?<object>.*?)\.\).*?for
```

This Base-rule will match the pattern of the log and will capture the following data:
- **SQLISPackage100** into the `<tag2>` field  
- **12289** into the `<vmid>` field  
- **Information** into the `<severity>` field  
- **LRVM-XM** into the <`dname>` field  
- **NTAUTHORITY** into the`<domain> `field  
- **SYSTEM** into the `<login>` field 
- **LR Backup Cleanup** into the `<object>` field
***
#### MPE Rule Processing Logic

When creating new MPE rules (both Base and Sub-rules), they are automatically sorted in a processing order based on creation time — the first created is processed first.
- Each rule set has its own processing order for Base and Sub-rules.
- The **Sorter prioritizes more descriptive rules** first and leaves general or catch-all rules for last.
- **Custom rules always take priority** over system (LogRhythm-created) rules:
    - Custom Base-rules run before system Base-rules.
	    - Custom Sub-rules where the value parsed for VMID should be a specific value by Sort Order
		- All other Custom Sub-rules by Sort Order
    - Custom Sub-rules run before system Sub-rules.
		- Custom Sub-rules where the value parsed for VMID should be a specific value by Sort Order 
		- System Sub-rules where the value parsed for VMID should be a specific value by Sort Order 
		- All other Custom Sub-rules by Sort Order 
		- All other System Sub-rules by Sort Order
- **Sub-rule Sorting**
	- **Sorting only matters** for Sub-rules using **wildcards or RegEx**.
	- If all tag values are **explicitly defined**, sorting doesn't affect matching.
**MPE processes Sub-rules in this order:**
1. Custom Sub-rules with specific VMID.
2. System Sub-rules with specific VMID.
3. Custom Sub-rules without specific VMID.
4. System Sub-rules without specific VMID.
> Wildcard-heavy Sub-rules should be placed **last** to avoid overshadowing more specific rules.

**Changing the Sort Order**
- **LogRhythm auto-sorts MPE Rules**, but sorting behavior can be customized.
- **System and Custom Sub-rules are always statically sorted.**
- For **Custom Base-rules**, you can:
    - Choose **static or auto-sorting**.
    - Enforce **relative order** among auto-sorted custom rules.
    - **Override** sort position relative to system Base-rules.
    - Set a rule to sort **above all system rules**.
**Why it's important:**
- Sorting affects rule execution; one rule might **prevent others from processing**.
- Use sorting control to ensure **priority and accuracy**.
- **Avoid Catch-all rules** running too early; they may suppress more specific rules.
**Recommendation:**
- If multiple Base-rules cover the same log types, consider **merging them with better Sub-rules** instead of relying on sort order.
#### Rule Library Browser
- After creating new MPE Rules, you can **view them in the Rule Library**.
- The Rule Library includes **over 7,000 System MPE Rules** provided by LogRhythm.
- These rules are part of the **LogRhythm Knowledge Base (KB)**.
- MPE Rules are organized and **listed by Log Message Source Type**.

#### MPE Policy Settings
**MPE Policy (Log Processing Policy)**
- An **MPE Policy** is a **collection of MPE Rules** tied to a specific **Log Source Type** (e.g., Cisco PIX, Windows Security Logs).
- It controls **how logs are processed**, including:
    - **Which MPE rules** are applied.
    - **Time to Live (TTL)** for logs.
    - Whether logs are **archived or forwarded**.
#### MPE Rule vs. MPE Policy

| Aspect       | MPE Rule                  | MPE Policy                                    |
| ------------ | ------------------------- | --------------------------------------------- |
| Purpose      | Parses metadata from logs | Manages log/event handling & rule application |
| Scope        | Specific parsing logic    | Broader processing logic for log source type  |
| Editable via | MPE Policy Rule Editor    | Deployment Manager > MPE Policy Editor        |
#### Key Points
- **Only logs assigned an MPE Policy are processed** by MPE.
- LogRhythm provides **default MPE Policies** (`LogRhythm Default`, possibly `V2.0` in optimization project).
- A Log Source can have:
    - **Only one MPE Policy**
    - A policy that **matches the Log Source Type**
#### Modifying MPE Policies
1. Open **Client Console** with admin access.
2. Go to **Deployment Manager > Log Processing Policies tab**.
3. Select a policy → `File > Properties` → Opens **MPE Policy Editor**.
4. Enable/disable rules, edit TTL, event forwarding via **MPE Policy Rule Editor**.
5. Use **GLPR Manager** (Global Log Processing Rule Manager) for global changes:
    - Found in **Deployment Manager > Tools > Administration > GLPR Manager**.
#### Best Practice
- Avoid placing Catch-all rules too early.
- Use GLPR Manager for consistent, organization-wide processing behavior.
***
#### Log Source Type Manager
- LogRhythm has **predefined Log Source Types** for hundreds of devices.
- For **custom applications or new devices**, you may need to **create a custom Log Source Type** before building MPE Rules.
#### Accessing the Manager
- Found in **LogRhythm Console** via:  
    `Tools > Knowledge > Log Source Type Manager`
#### Steps to Create a Custom Log Source Type
##### Step 1: Create New Log Source Type
- Click **New** → opens **Log Source Type Properties**.
- Assign:
    - **Name**
    - **Log Format** (critical for parsing):
        - **Syslog**: Logs sent via Syslog protocol.
        - **Flat File/Text File**: Plain text logs (no structured standard).
> Choosing the wrong log format results in logs marked as **Unidentified** and unprocessed by MPE.
##### Step 2: Save the Log Source Type
- After verifying log format and naming, click **OK**.
#### Tips & Naming Conventions
- Use logical names starting with **Collection Method + Device/Vendor**:
    - `API - Office 365 Management Activity`
    - `Syslog - Palo Alto Firewall`
    - `Flat File - Microsoft IIS (IIS Format) File`
#### Additional Help
- Refer to **Device Configuration Guides** for collection-type-specific Log Source Types.
#### Best Practice
- Always match the **log format** to your device's log generation method to ensure proper parsing and identification.
***
### Date Format Manager
- The **Date Format Manager** is used to define how LogRhythm extracts **date/time** from log messages, using **RegEx-based patterns**.
- Required when adding **custom Log Sources**, especially for **Flat Files** or logs with non-standard formats.
#### How It Works
- LogRhythm matches the **date pattern** using pre-defined **Date Formats** based on RegEx-like tokens.
- You can:
    - **Select an existing Date Format** from a similar device type.
    - **Create a custom format** if no match exists.
#### Example
**Sample Log Message:**
`06/28/2017 02:41:06 PM alert action: 1-28255 Win.Trojan.Kuluoz Detected sHost:150.94.138.231 dHost:195.190.211.155 email: frederick@cert.logrhythm.com`

**Example**
This is the associated Date Format used by LogRhythm to parse the date from the Flat File log message:
`<M>/<d>/<yy> <h>:<m>:<s> <t>`
#### Date Format Tokens

|Token|Matches|
|---|---|
|`<M>`|Month (1 or 2 digits)|
|`<d>`|Day (1 or 2 digits)|
|`<yy>`|Year (2 or 4 digits)|
|`<h>`|Hour|
|`<m>`|Minute|
|`<s>`|Second|
|`<t>`|AM/PM|

> These tokens **replace typical formats** like `MM/DD/YYYY HH:MM:SS`.

#### Best Practice
- If no existing Date Format matches your log’s timestamp:
    - **Manually define** one using the **Help tab** in the Date Format Properties.
- Always verify parsing accuracy to avoid **unidentified or improperly timed logs**.
***
#### Date and Time Parsing
- If **Date/Time issues** occur, verify the **Log Source Type settings** are correct.
- When selecting a Log Source Type, the **associated Date/Time RegEx** is applied automatically.
- For **Flat File logs**, you **must manually select** the correct date/time format due to their varied structure.
- LogRhythm offers **many predefined formats** to choose from.
#### Important for MPE Rules
- **Do NOT include date/time** in the **Base-rule RegEx**.
- LogRhythm **automatically extracts and normalizes** time during processing.
- This ensures **Time Normalization** and accurate log correlation.
***
### Steps After MPE Rule Creation

1. **Set Rule to Test Mode**
    - Change status of Base-rule and Sub-rules to Test in the Rule Builder.
2. **Assign a Log Processing Policy**
    - Review existing policies in Deployment Manager.
    - Select the correct Log Source Type.
    - Create a new policy if needed and associate it with the correct Log Source Type.
3. **Enable the Rule**
    - Edit the Log Processing Policy properties.
    - Enable the rule and set the Risk-Based Priority (RBP).
4. **Test the Rule**
    - Verify the rule is successfully processed by MPE.
    - If not, adjust the rule and retest.
5. **Edit Rule Sorting**
    - Place detailed and specific rules at the top.
    - Place general or Catch-all rules at the bottom.
6. **Promote to Production**
    - Change the rule status from Test to Production in the Rule Builder.
7. **Re-enable MPE Rule in Policy**
    - In the Log Processing Policy, ensure the new custom rule is enabled and RBP is set.
8. **Verify Parsing**
    - Perform an Investigation or check the Dashboard to confirm logs are identified and metadata is parsed correctly.
9. **Refine if Needed**
    - Make necessary changes and repeat verification until logs are processed as expected.
10. **Submit to LogRhythm Labs (Optional)**
	- If the rule is reusable, submit it to LogRhythm Labs for review and potential inclusion in the official content.
***
### Exercises Part 4
**How to get Logs that need to parse**
- Open Investigation Tap > Create New Investigation > add Log Source Type > Specific EVID for Log Source
**The Rule Library**
- Contain Created MPE Rules.
- And Use To find and edit MPE Rules
- If you want to see you custom rule open Rule Library > View > Show development Rule and show custom rule. and from this we can change status of rule from development to production.
**To Change status of associated sub-rules**
-  R_Click in the list of sub rules and select **Sync with base rule** then Rule status "to be as rule status in production" 
**Enable Rule in LogRhythm**
1. Open the Deployment Manager. 
2. Select the **_Log Processing Policies_** tab. 
3. Double-click to select the **Syslog - LogRhythm Syslog Generator Log Source Type**. 
4. The **MPE Policy Editor** appears. 
5. Click to put a checkmark the box in the **Edit** field for each of your new rules so you can include them in the MPE Policy. 
    1. Right-click and select **Properties** from the context menu. 
6. The **MPE Policy Rule Editor** appears. 
    1. Click to put a checkmark the **Enabled** box. 
        1. All edits you make will apply to all the rules that are currently selected. 
7. Select **OK**. 
8. You'll return to the **MPE Policy Editor** window. 
9. Select **OK**.

**Verify logs are identified and parsed in LogRhythm**
1. From the LogRhythm Client Console, select the **Investigate** button. 
2. Select **Configure New Investigation** to find all logs in the last hour from the Log Source Type: **Syslog - LogRhythm Syslog Generator** 
3. When the results are displayed, select the **Log Viewer** tab to review the logs. 
    1. Review the **MPE Rule Name field**. Make sure your new MPE Rule has been applied to the appropriate log messages. 
    2. Review the following **metadata fields** to ensure they are populated correctly for the different logs and their MPE Rules (not all fields will contain data for every log) 
        1. Common Event 
        2. MPE Rule Name 
        3. Host (Origin) and (Impacted) 
        4. Vendor Message 
        5. User (Origin) and (Impacted) 
        6. Object 
        7. Session ID 
        8. Group 
        9. Process 
        10. TCP/UDP Port (Origin) and (Impacted) 
        11. Protocol 
        12. Packets Received or Packets Sent 
        13. Amount 
        14. Account 
        15. Sender 
        16. Recipient 
        17. Subject
***
### RegEx Best Practices

**Rule Performance**
- Use **literal characters** instead of general matching characters for better performance, and use **character sets [ ]** when possible.
- When testing RegEx in the Rule Builder, try it on **a large volume of logs** with **different formats** to ensure good performance with both matching and non-matching logs.
- Use **non-capturing groups (?: )** to avoid storing data in memory and improve processing speed.
    - **Example:** Windows has two versions of related logs: one for `VMID0008` and one for `VMID0009`. If you only need the VMID number and not the MESG text, use `(?:MESG)` to verify its presence without capturing it, ensuring the data is matched but not stored in memory.
- Avoid using **\x** for matching hexadecimal values as it can slow performance; instead, use **.*?** to skip over such data more efficiently.

**Reviewing Processing Performance**
**LPS.log**
- **LPS Detail Logs** are generated by the **MPE** component of the Mediator Server service to record how often a rule is compared to a log message and the total processing time for each rule.
- The **LPS Detail report** provides detailed statistics for a specific Log Processing Policy over a set period, shown in a standard text format viewable in any text viewer.
- It helps identify log processing performance issues and pinpoint problematic rules.
- The report includes:
    - **Header**: Report info, creation date/time, and license ID for the Mediator Server service.
    - **Policy sections**: Data field headers, log source type, policy name, and one line for each Base-rule in that policy.
- Four key columns in the report are especially useful for analyzing **MPE Rule processing performance**.
**SCMEDSVR Logs**
The scmedsvr.log file contains errors, warnings, and data pertaining to System Monitor agent connections, and network operations. This log can be examined to review:
- The number of logs inserted per second into the Indexer by the scmedsvr service
- The number of logs processed by the MPE per second
- The number of logs currently in the MPE Log Processing queue
- The percentage full of the Log Processing queue (logs not yet processed)
- In **LogRhythm v7.2.x**, a feature was introduced to **timeout poor-performing rules**, causing affected logs to fall to **Catch-all rules**.
- Purpose: Helps identify slow rules and collect sample logs to send to Support.
- To check if it’s enabled, look for this line in `scmedsvr.ini`:
    `[Optional] MPERuleTimoutEnabled=True`
- If not enabled, consult LogRhythm Support to review pros and cons before activating.
- If enabled:
    1. Run an **Investigation** for logs with **Common Event = LogRhythm MPE Rule Performing Poorly**.
    2. Review results to identify offending rules and related log messages.
    3. Watch for **repeat offenders** (a few timeouts don’t always mean a bad rule).
    4. Save sample logs in an **LLX file** and open a Support ticket for performance improvement recommendations.
**scmpe.log**
- **scmpe.log** stores errors, warnings, and data related to the **MPE** component of the server.
- Check this file for **error messages** tied to log processing issues.
- If you find **repeat offending logs** that lock threads or cause timeouts, possible causes include:
    - Log issues: format changes, very large log sizes, or excessive noise.
    - MPE Rule issues: poorly performing RegEx, incorrect rule sort order, or outdated RegEx needing updates.

**Efficiency**
- **Poorly written RegEx** can cause slow processing in LogRhythm; understanding the RegEx engine is key to improving efficiency.
- **How the RegEx engine works:**
    - Reads input **left to right**.
    - After finding a match, keeps scanning for more matches.
    - If no match is found, it **backtracks** to try other paths.
    - **Index**: Position tracker in the string (starts at 0).
    - **Backtrack**: Happens often with `.*?` as it creates multiple match paths, which can hurt performance.
- **Performance tip:** If a reliable literal or delimiter exists in the log, design the RegEx around it rather than using `.*?`.
- **Examples:**
    - `,[^,]*,` performs better than `,.*?,` in comma-delimited logs because it avoids excessive backtracking.
    - `.*?` and `[^*]+` may return the same result, but `[^*]+` is more efficient due to zero backtracking.
- **Character sets** are highly effective for predictable delimiters and are naturally non-greedy:
    - Examples: `,[^,]*,` or `\|[^\|]*\|` work well with quantifiers.

**RegEx Recommended Practices**

| **Name**                              | **Recommended Pattern**                                                                              | **Description**                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| ------------------------------------- | ---------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Negative Character Class**          | `"[^"]*"` for double quote delimiters`,[^,]*,` for comma delimiters`\|[^\|]*\|` for pipe delimiters. | Can be used for any type of predictable delimiter.Use negative character classes in log messages with clear delimiters, such as quotation marks, commas, or pipes. This will match any character that is **not** the delimiter. This can greatly improve parsing performance vs. a more generic match, such as `.*?`.**Example:** `MsgID=1590.*?user\s"(?<account>[^"]*)"`                                                                                                                    |
| **Non-greedy Match**                  | `.*?`                                                                                                | If you need to match any characters until a specific set of characters appears, use this pattern.**Example:** `MsgID=1590.*?user\s"(?<account>[^"]*)"`                                                                                                                                                                                                                                                                                                                                        |
| **Overloading Map Tags**              | `(?<[map tag]>[regex])`                                                                              | Map tags should almost always be overloaded. The default RegEx for map tags is `.*` which will match everything to the end of the log.**Example:** `MsgID=1590.*?user\s"(?<account>[^"]*)"\s(?<tag1>.*?)$`                                                                                                                                                                                                                                                                                    |
| **Preceding and Trailing Values**     | N/A                                                                                                  | Always match as much constant text as possible. The more information the RegEx has to evaluate, the faster it will be at identifying non-matching logs. For any parsed field, it is best to search for a constant value before and after the value being parsed.**Example:** `MsgID=1590.*?user\s"(?<account>[^"]*)"\s(?<tag1>.*?)$`                                                                                                                                                          |
| **Look Aheads**                       | `(?=[regex])[regex]``(?![regex])[regex]`                                                             | Positive and negative look aheads allow for an initial check in the RegEx to see if a case is satisfied in the log messages. These are useful for finding a value later in a log to reduce extraneous processing for non-matching logs. **Do not** use for values that appear very early in a log message, such as just past a Syslog header. Look ahead is more costly than regular expressions if the match is always found early.**Example:** `(?=.*?match contains this phrase)\s<sip>\s` |
| **Multiline Character Match Pattern** | `[\r\n]`                                                                                             | Using a character class containing both `\r` (return) and `\n` (newline) allows for either multiline character to appear as well as both in either order. Some log messages vary in the order of these new lines and others only contain a newline.                                                                                                                                                                                                                                           |
| **Narrow Character Classes**          | `[a-z0-9_]+`                                                                                         | The shorthand character class `\w` matches Latin alphabet characters, Hindu-Arabic numerals (0-9), underscores, and other script supported in Unicode. Narrowing the match to only the relevant character set will yield better performance.                                                                                                                                                                                                                                                  |

**The Whole Process for Custom MPE Rules**
The following steps can be used as a reference when creating new Custom MPE Rules for use in your LogRhythm deployment.
1. **Research** your custom Log Source.
    - Log collection method
    - Log message format
    - Log and severity levels
    - Determine what Event and Alarms are needed for the logs
2. Verify a **Log Source Type** exists.
    - Review the Log Source Type Manager for a list of existing Log Source Types.
    - Assign the new MPE Rule to a valid Log Source Type or create a new Custom Log Source Type, if one does not already exist.
    - Assign the correct Log Format that correctly matches the log collection method.
        - Syslog
        - Text File
3. Create your new **Custom MPE Rule**.
    - Create a new MPE Rule for your custom Log Source
        - Name the rule something specific.
        - Assign a Common Event from the list of over 29,000 available options.
    - Create a Base-rule and test your RegEx for successful matching results.
        1. Create Sub-rules and test for successful matching results.
            - Assign a Common Event for each Sub-rule.
    - Create a Catch-all rule for unknown logs that might be generated by this Log Source.
4. Change the Rule Status, of your new Base-rule and its associated Sub-rules, to **Test**.
5. Assign a **Log Processing Policy**.
    - Review the list of existing Log Processing Policies in the Deployment Manager.
    - Select the appropriate Log Source Type.
    - Create a new Log Processing Policy if one is not already created.
    - Give it a name and select the appropriate Log Source Type.
6. **Enable** the new MPE Rule.
    - Edit the properties of the Log Processing Policy.
    - Select the Enable option and set the Risk Based Priority value.
7. **Verify** it was tested successfully by the MPE.
    - If not, make changes to the rule for improved efficiency.
        - Enable to Test again.
8. **Edit Base-rule Sorting**.
    - Detailed rules should be on top and more general rules on the bottom.
    - Catch-all rules should be at the bottom of all other rules.
9. Change the Rule Status to **Production** in the Rule Builder.    
10. **Enable** the new MPE Rules.
    - Edit the properties of the Log Processing Policy.
    - Enable the new Custom MPE Rules and set the Risk Based Priority value.
        - Your MPE Rules and your Log Processing Policy are associated to your custom Log Source Type.
11. **Verify** the logs are identified and metadata is parsed by the rule as expected.
    - Perform an Investigation or review the logs in the Dashboard.
12. **Make changes**, if needed, to ensure the log is processed as desired.
    - Repeat verification of log processing and make changes until log is processed as desired.
13. If you've just created a new MPE Rule for a device that will likely be used by others, **submit it to the LogRhythm Labs team for review**.
    - If approved, the Labs team can add it permanently to the list of available Log Source Types, MPE Rules, and perhaps create some additional Sub-rules made available to all customers.
        - The LogRhythm Labs engineers can review your logs and MPE Rules at no cost. However, their time and resources are limited, and there is no guaranteed time frame for response to your request.
        - The LogRhythm Professional Services (PS) engineers can assist with writing RegExes for custom Log Sources in a timelier manner. However, their time is billable.
