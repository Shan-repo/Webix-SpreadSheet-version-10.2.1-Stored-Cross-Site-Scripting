Webix SpreadSheet version 10.2.1 is vulnerable to Stored Cross-Site Scripting (XSS) in the comments and graph components. This vulnerability occurs when user input is not properly sanitized, allowing malicious scripts to be embedded and stored within the web spreadsheet. When a user interacts with the compromised components, these scripts are executed in their browser, potentially leading to data theft, session hijacking, and unauthorized actions.

Affected Components:
- Comments: User comments added to cells in the spreadsheet are susceptible to XSS if inputs are not properly sanitized.
- Graph Components: Titles, labels, or other text fields within the graphing functionality are also vulnerable, allowing malicious scripts to be stored and executed.

Technical Details:

1. Stored XSS in Comments:
   - The comments feature allows users to insert comments into spreadsheet cells.
   - If an attacker inputs a script (e.g., `"><img src=1 onerror=alert(JSON.stringify(localStorage))>`) into a comment, it is stored in the application’s backend without proper sanitization.
   - When other users view or interact with the comment, the malicious script is executed in their browser.

2. Stored XSS in Graph Components:
   - The graphing feature allows users to add titles and labels to graphs within the spreadsheet.
   - An attacker can inject a script into these text fields, which is then rendered on the graph without sanitization.
   - This stored script runs in the context of any user who views the graph, leading to potential exploitation.
     
  ![image](https://github.com/user-attachments/assets/482997b8-782e-47c6-b794-0995a58d6146)


Impact:
- Session Hijacking: Attackers can steal session cookies, gaining unauthorized access to user accounts.
- Data Exfiltration: Sensitive data can be captured and exfiltrated by the malicious script.
- Phishing: Scripts can be used to present fake login forms, tricking users into revealing credentials.
- Defacement: The application’s interface can be altered, misleading users or damaging the application's integrity.

Exploitation Example:

1. Steps to Exploit:
   - Insert a script payload into the comment section of a cell or into a graph label, such as:  
   &quot;&gt;&lt;img src=1 onerror=alert(JSON.stringify(localStorage))&gt;

     
   - Save the comment or graph settings.
   - The script is stored and triggered when any user accesses the spreadsheet and views the comment or graph.

2. Proof of Concept (PoC):
   - Add the following payload in a comment or graph title:  
    &quot;&gt;&lt;img src=1 onerror=alert(JSON.stringify(localStorage))&gt;
     
   - When the victim interacts with the infected component, the script executes, sending their cookies to the attacker's site.

Mitigation Recommendations:

1. Input Sanitization and Output Encoding:
   - Ensure that all user inputs are sanitized before being stored and encoded before rendering in the browser.
   - Use libraries or built-in frameworks to sanitize inputs, removing dangerous characters and preventing script execution.

2. Content Security Policy (CSP):
   - Implement a strong Content Security Policy to restrict the execution of unauthorized scripts, which can significantly reduce the impact of XSS attacks.

3. Validation and Escaping:
   - Apply strict validation rules on user inputs and escape outputs before displaying them in the application.


Authors:<br>
Shanavas Shakeer <br>
Patrick Dean Ramos <br>
Nathu Nandwani <br>
Kevin Rosales <br>
