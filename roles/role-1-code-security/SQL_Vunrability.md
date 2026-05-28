user = db.execute(
    f"SELECT * FROM admin_users WHERE username = '{username}' AND password = '{password}'"
).fetchone()

>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

user = db.execute(
    "SELECT * FROM admin_users WHERE username = ? AND password = ?",
    (username, password)
).fetchone()

# The vulnerable f-string login query was replaced with a parameterized query. This ensures usernames and passwords are treated as data instead of executable SQL, preventing authentication bypass through SQL injection.

============================================================================

db.execute(
    f"UPDATE admin_users SET last_login = datetime('now') WHERE username = '{username}'"
)

>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

db.execute( 
    "UPDATE admin_users SET last_login = datetime('now') WHERE username = ?", 
    (username) 
)

# The username value was removed from the f-string and replaced with a parameterized query. This prevents attackers from injecting malicious SQL through the username field while keeping the update functionality unchanged.

============================================================================

query = ( f"SELECT orders.*, customers.name, customers.email " f"FROM orders JOIN customers ON orders.customer_id = customers.id " f"WHERE orders.id = {order_id}" ) order = db.execute(query).fetchone()

>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

query = ( "SELECT orders.*, customers.name, customers.email " "FROM orders JOIN customers ON orders.customer_id = customers.id " "WHERE orders.id = ?" ) order = db.execute(query, (order_id,)).fetchone()

# The f-string query was replaced with a parameterized query using ?. This prevents user input from being interpreted as SQL code and protects the application from SQL injection attacks.