## 9. Define Errors Out of Existence

### 9.1. Why Exceptions Cause Complexity

**An exception** is any uncommon situation that changes the normal control flow of a program. This includes both formal exception mechanisms such as throwing/catching exceptions and special return values.

**Sources of exceptions:** Invalid configuration parameters from callers, I/O errors or resource shortages, lost or delayed packets in distributed systems, or the detection of internal logic errors.

**Why exceptions increase complexity:**
  - **Difficult handling:** It is complex to either keep the system running after an error or abort the operation and restore the system to a consistent state.

  - **Potential for new errors:** Error-handling code can introduce secondary exceptions that are even more complex than the original error. For example, a delayed packet may cause the system to send a duplicate packet.

  - **Verbose syntax:** `try-catch` blocks are often cumbersome and require more lines of code than the normal execution path. They interrupt the main flow of the code and make it harder to identify where the actual error occurs.

  - **Difficult testing:** Errors such as I/O failures are difficult to simulate, so error-handling code may rarely be executed and can easily contain hidden bugs.

Many programmers are influenced by **defensive programming**, believing that detecting and reporting as many errors as possible is always better. This can lead to the overuse of exceptions and the creation of many unnecessary error cases.

  => **Consequence of overuse:** This design increases system complexity and makes class interfaces more complicated and **shallower**.

**Shifting responsibility to the caller:** Programmers often use exceptions to avoid dealing with difficult situations themselves instead of finding a more fundamental solution. Throwing an exception upward with the justification of "letting the caller decide how to handle it" only increases the burden and complexity of the entire system because the caller often does not know what to do either.

=> **The fundamental nature of complexity:** Throwing an exception is easy, but handling it is difficult. Exceptions can propagate upward through multiple layers of the system. Therefore, the best way to reduce complexity is to minimize the number of places where exceptions must be handled.

=> The best way to eliminate complexity in error handling is to **design APIs so that there are no exceptions that need to be handled**.

---

### 9.2. Use Cases for Eliminating Error Complexity

#### Deleting Files in Windows vs. Unix

**The problem in Windows:** Windows does not allow a file to be deleted if it is currently open by another process. This causes the operating system to report an error. The user or programmer must find and terminate the process holding the file, and in some cases may even need to restart the machine before the file can be deleted.

**The Unix solution — Define the error out of existence:** 
- When a request is made to delete an open file, Unix does not report an error. Instead, it marks the file for deletion and immediately reports success.

- The file name is removed from the directory, so other processes can no longer see it. However, the existing file data remains available to processes that already have the file open, allowing them to continue reading or writing normally.

- The file is completely destroyed only after all processes that have it open close it.

- This approach eliminates two error scenarios entirely:

1. The error caused by attempting to delete a file that is currently in use.
2. The interruption of processes that are currently accessing the file.


#### Java's `substring` Method

**The problem in Java:** Java's `substring` method requires two indices: a start index and an end index. If either index is outside the valid range of the string, the method throws an `IndexOutOfBoundsException`. This forces programmers to write an additional 5–10 lines of code just to validate the indices and adjust them to a valid range before calling the method.

**Alternative solution — Define the error out of existence:**

The method would be easier to use and **deeper** if it automatically adjusted the indices:

> "Return the characters within the requested index range that actually exist in the string."

With this definition, if an input index is negative or greater than the string's length, the method can still operate normally, returning an empty string or the matching portion of the string instead of throwing an exception.

Other languages, such as Python, use a similar error-free approach when slicing strings or arrays.

---

### 9.3. Masking Exception Information

**Exception masking** is a technique for detecting and handling exceptional conditions at lower layers of the system.

  => This allows higher layers of the software to remain completely unaware that the error occurred, thereby reducing the number of places where exception-handling code is required.

**Examples:**

  - **TCP:** TCP automatically retransmits lost packets as part of its own operation. As a result, applications at higher layers can continue operating normally without needing to know that packet loss occurred.

  - **NFS:** When an NFS server fails, the file-system code on the client can continuously retry requests until they succeed instead of propagating the error to the application. The application may temporarily block and then automatically continue when the server becomes available again, without requiring explicit error-handling code.

**Design benefit:** This approach creates **deeper classes** by narrowing the interface exposed to higher layers. Fewer exceptions are exposed, while the complexity of handling them is pushed down into lower layers where they can be handled more effectively.

---

### 9.4. Exception Aggregation

Exception aggregation means handling multiple exceptions with a single piece of code. Instead of writing separate handlers for each exception, handle them all in one common location using a shared error handler.

 **Example: Web Server**

- Many students, when designing a method for retrieving parameters such as `getParameter`, wrapped each call in a separate `NoSuchParameter` exception handler.

- This resulted in a large amount of duplicated error-handling code.

=> A better approach is to allow these exceptions to propagate upward to the web server's highest-level **dispatch method**.

At this level, a single handler can catch all of these exceptions and generate the appropriate error response.

```text
Request
   ↓
Dispatch Method
   ↓
Application Logic
   ↓
getParameter()
   ↓
NoSuchParameter
   ↑
   │
Centralized Error Handler
   ↓
Error Response
```
This avoids duplicating the same error-handling logic throughout the application.

### 9.5. Just Crash

For errors that are extremely rare and from which the system cannot realistically recover by itself, such as out-of-memory errors or disk hardware failures, the simplest and often best approach is to output diagnostic information and immediately terminate the application.

Forcing every line of code to check for these unrecoverable errors, such as checking the return value of malloc in C, only increases code complexity unnecessarily and makes it easy to overlook error checks.

**Create a wrapper:** Instead of calling potentially failing operations directly, use a wrapper that automatically checks for errors and deliberately terminates the program when an unrecoverable error occurs.

**Example: Create a ckalloc wrapper around malloc**

```text
malloc()
   ↓
ckalloc()
   ↓
Check result
   ├── Success → return memory
   └── Failure → report error + terminate
```

**Important exception**: Whether an application should simply crash depends on the nature of the system.

For distributed storage systems, for example, the system may be required to implement sophisticated error-handling and recovery mechanisms to restore data from replicas instead of terminating the system.

### 9.6. Design Special Cases Out of Existence

The problem with special cases: Special cases cause source code to become filled with if statements, making the code harder to understand and more error-prone.

=> The best way to eliminate special cases is to design the normal case so that it automatically handles special cases without requiring separate code.

**Example: Selection in a Text Editor**

Instead of using a state variable to check whether there is no selection and then writing separate handling code, design the system so that a selection always exists. When no text is selected, represent it as an empty selection whose start and end positions are the same.

With this approach, operations such as copying or deleting an empty selection automatically behave correctly according to the normal logic:

- Copying an empty selection → copies 0 bytes.
- Deleting an empty selection → leaves the existing text unchanged.
- No special conditional checks are required.

The concept of "no selection" may be important to the user interface, but it does not necessarily need to be represented directly in the underlying code.

**This illustrates the principle:** Different layers can have different abstractions.

### 9.7. Taking It Too Far
Defining errors out of existence or masking exceptions is only appropriate when the information about the error is genuinely unimportant and unnecessary for higher layers of the system.

**Consequences of Overuse — Networking Module Example**
A group of students attempted to hide all network errors. When an error occurred, the module caught it, ignored it, and continued running as if nothing had happened.
This prevented the higher-level application from knowing whether a packet had been lost or whether the remote server had gone down.

As a result, it became impossible to build a stable and reliable application because the application could not make appropriate decisions based on important network failures.

=> Unimportant information should be hidden. However, when error information is important for higher-level logic, it must be exposed through the API, even if doing so increases the complexity of the interface.

The goal is not to hide every error. The goal is to hide errors that the higher layers do not need to know about.