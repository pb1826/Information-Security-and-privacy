# Information-Security-and-privacy
This assignment will help you understand security mechanisms. You will be guided through the steps of creating a reference monitor using the security layer functionality in Repy V2. A reference monitor is an access control concept that refers to an abstract machine that mediates all access to objects by subjects. This can be used to allow, deny, or change the behavior of any set of calls. While not a perfect way of validating your reference monitor, it is useful to create test cases to see whether your security layer will work as expected. (The test cases may be turned in as part of the next assignment.)

This assignment is intended to reinforce concepts about access control and reference monitors in a hands-on manner.
n this assignment you will create a security layer which keeps a backup copy of a file in case it is written incorrectly. This is a common technique for things like firmware images where a system may not be able to recover if the file is written incorrectly. For this assignment, every `correct' file must start with the character 'S' and end with the character 'E'. If any other characters (including lowercase 's', 'e', etc.) are the first or last characters, then the file is considered invalid.

However, you must permit the application to write information into the file.
The application should not be blocked from performing any writeat() operation, because when it chooses it may later write 'S' at the start and 'E' at the end. Note that checking if the file starts with 'S' and ends with 'E' is only performed when close is called.

You may store two copies of A/B files on disk, one that is the valid backup (which is used for reading) and the other that is written to. When an app calls ABopenfile(), this indicates that the A/B files, which you should name filename.a and filename.b, should be opened.
When the app calls readat(), all reads must be performed on the valid file. Similarly, when the app calls writeat(), all writes must be performed on the invalid file. If the app uses ABopenfile() to create a file that does not exist (by setting create=True when calling ABopenfile()), the reference monitor will create a new file 'SE' in filename.a and an empty file called filename.b. When close() is called on the file, if a file is not valid, it is discarded. if both files are valid, the older one is discarded.

Note that the behavior of other file system calls should remain unchanged. This means listfiles(), removefile(), and calls to files accessed with openfile() instead of ABopenfile() remain unchanged by this reference monitor.

Three design paradigms are at work in this assignment: accuracy, efficiency, and security.

    Accuracy: The security layer should only stop certain actions from being blocked. All other actions should be allowed. For example, if an app tries to read data from a valid file, this must succeed as per normal and must not be blocked. All situations that are not described above must match that of the underlying API.

    Efficiency: The security layer should use a minimum number of resources, so performance is not compromised. For example, keeping a complete copy of every file on disk in memory would be forbidden.

    Security: The attacker should not be able to circumvent the security layer. Hence, if the attacker can cause an invalid file to be read or can write to a valid file, then the security is compromised, for example.

