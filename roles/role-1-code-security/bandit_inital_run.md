darren_webdev@axiom-01:/opt/bpbpies$ bandit -r . -x venv,.venv,env
[main]  INFO    profile include tests: None
[main]  INFO    profile exclude tests: None
[main]  INFO    cli include tests: None
[main]  INFO    cli exclude tests: None
[main]  INFO    running on Python 3.12.3
Run started:2026-05-28 18:21:31.076667+00:00

Test results:
>> Issue: [B608:hardcoded_sql_expressions] Possible SQL injection vector through string-based query construction.
   Severity: Medium   Confidence: Low
   CWE: CWE-89 (https://cwe.mitre.org/data/definitions/89.html)
   More Info: https://bandit.readthedocs.io/en/1.9.4/plugins/b608_hardcoded_sql_expressions.html
   Location: ./app.py:155:10
154         query = (
155             f"SELECT orders.*, customers.name, customers.email "
156             f"FROM orders JOIN customers ON orders.customer_id = customers.id "
157             f"WHERE orders.id = {order_id}"
158         )

--------------------------------------------------
>> Issue: [B608:hardcoded_sql_expressions] Possible SQL injection vector through string-based query construction.
   Severity: Medium   Confidence: Medium
   CWE: CWE-89 (https://cwe.mitre.org/data/definitions/89.html)
   More Info: https://bandit.readthedocs.io/en/1.9.4/plugins/b608_hardcoded_sql_expressions.html
   Location: ./app.py:214:14
213             user = db.execute(
214                 f"SELECT * FROM admin_users WHERE username = '{username}' AND password = '{password}'"
215             ).fetchone()

--------------------------------------------------
>> Issue: [B608:hardcoded_sql_expressions] Possible SQL injection vector through string-based query construction.
   Severity: Medium   Confidence: Medium
   CWE: CWE-89 (https://cwe.mitre.org/data/definitions/89.html)
   More Info: https://bandit.readthedocs.io/en/1.9.4/plugins/b608_hardcoded_sql_expressions.html
   Location: ./app.py:220:18
219                 db.execute(
220                     f"UPDATE admin_users SET last_login = datetime('now') WHERE username = '{username}'"
221                 )

--------------------------------------------------
>> Issue: [B105:hardcoded_password_string] Possible hardcoded password: 'bpbpies_secret_key_2024'
   Severity: Low   Confidence: Medium
   CWE: CWE-259 (https://cwe.mitre.org/data/definitions/259.html)
   More Info: https://bandit.readthedocs.io/en/1.9.4/plugins/b105_hardcoded_password_string.html
   Location: ./config.py:8:17
7       class Config:
8           SECRET_KEY = 'bpbpies_secret_key_2024'
9           DB_PASSWORD = 'freddibnah1938'

--------------------------------------------------
>> Issue: [B105:hardcoded_password_string] Possible hardcoded password: 'freddibnah1938'
   Severity: Low   Confidence: Medium
   CWE: CWE-259 (https://cwe.mitre.org/data/definitions/259.html)
   More Info: https://bandit.readthedocs.io/en/1.9.4/plugins/b105_hardcoded_password_string.html
   Location: ./config.py:9:18
8           SECRET_KEY = 'bpbpies_secret_key_2024'
9           DB_PASSWORD = 'freddibnah1938'
10          DATABASE = 'database/bee_pies.db'

--------------------------------------------------

Code scanned:
        Total lines of code: 365
        Total lines skipped (#nosec): 0

Run metrics:
        Total issues (by severity):
                Undefined: 0
                Low: 2
                Medium: 3
                High: 0
        Total issues (by confidence):
                Undefined: 0
                Low: 1
                Medium: 4
                High: 0
Files skipped (0):
