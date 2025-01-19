# MYSQL-Notes


## Transaction States in DBMS
A transaction is a series of operations or tasks that work together to complete a specific process, which may involve changing data in a database. To handle issues like system failures, transactions go through different states.

A transaction state shows the current stage or condition of a transaction as it runs in the database. It tracks the progress and determines whether the transaction will finish successfully (commit) or stop due to an issue (abort).

## Different Types of Transaction States in DBMS

![image](https://github.com/user-attachments/assets/2e0ce364-5934-49b6-9d89-8d079975aae9)

### Simplified Explanation of Transaction States

1. **Active State**  
   - The transaction starts, and its instructions (like insert, update, delete) are being executed.  
   - Changes are not yet saved to the database but kept temporarily in memory.

2. **Partially Committed**  
   - The transaction finishes all its operations but hasn't saved the changes permanently to the database yet.  
   - If successful, it moves to the "Committed" state; if something fails, it moves to the "Failed" state.

3. **Failed State**  
   - If an error occurs during the transaction, it stops, and the system ensures the database remains consistent.  
   - The database recovery system may attempt to fix the issue or terminate the transaction.

4. **Aborted State**  
   - If recovery from the failed state isn’t possible, the transaction is rolled back (undone) to restore the database to its original state.  
   - The transaction may either be canceled or restarted.

5. **Committed State**  
   - The transaction successfully saves its changes permanently to the database.  
   - This marks the completion of the transaction.

6. **Terminated State**  
   - After committing or rolling back, the transaction ends, and the system is ready for a new transaction.

---

### Example: Bank Transaction ($500 transfer from Account A to Account B)
1. **Active State:**  
   The transaction starts and reads Account A’s balance ($1000). It checks if there are sufficient funds.  

2. **Partially Committed:**  
   Deducts $500 from Account A ($1000 - $500 = $500) and temporarily updates Account B’s balance ($1500).

3. **Committed:**  
   Changes are saved permanently:  
   - Account A = $500  
   - Account B = $1500  

4. **Failed State:**  
   If something goes wrong (e.g., system crash), the transaction stops.  
   - Example: $500 is deducted from Account A but not added to Account B.

5. **Aborted State:**  
   The failed transaction is rolled back:  
   - Account A = $1000  
   - Account B = $1000  

6. **Terminated State:**  
   The transaction ends after committing or rolling back. The system is ready for a new transaction.  

This process ensures the database remains consistent, either completing the transaction fully or undoing changes in case of failure.
