# Project 1d: Substitution Cipher

You are to implement a simple encryption and decryption system using a basic substitution cipher.

The script, `crypto_master.py`, should do one of two things depending on the arguments passed in (see hints for a discussion of "arguments"): 

## ENCRYPT

IF there is only 1 argument, assume that argument is the name of a file to ENCRYPT. Proceed as follows:

1. GENERATE a new, random substitution string variable named `key` containing all 26 letters of the alphabet in a random order (see hints).
2. Create an empty string variable called `encrypted_message`
3. Open the file specified as the first argument to the script (again, see hints)
4. Read the file one line at a time (i.e., for line in file)
5. For each line call an `encrypt` function you will write (see notes below). The basic alogrithm for the function is as follows:
    - strip() any extra spaces or newlines from the beginning or end of the line
    - create an empty string (function scope variable) called `encrypted_line`
    - now you want to iterate over each CHARACTER in the line. For each:
        - if the character is NOT an alphabetical character, just append the character directly to `encrypted_line`
        - if the character is an alphabetical character:
            - If the character is an `a` or an `A`, append the first character in your `key` string (i.e., `key[0]`) to `encrypted_line`
            - If the character is an `b` or an `B`, append the second character in your `key` string (i.e., `key[1]`) to `encrypted_line`
            - Hopefully you see the pattern...using ord() you can make this really easy by subtracting either 65 or 97 (`ord('A')` or `ord('a')`) and using that as your index into the `key` string, but you don't have to do it the easy way...
    - RETURN the `encrypted_line` string
6. Back in the main section, append the returned value to `encrypted_message` and repeat 4-6 until you've processed the whole message file
7. Save the full `encrypted_message` string to a file (see notes)
8. Save the `key` string to a file, as well (see notes)

## DECRYPT

IF there are 2 arguments, assume the first argument is the name of a file to DECRYPT and the second file contains the key used to decrypt the file. Preceed as follows:

1. Open and read the first and only line from the key file...store that string in a variable called `key`
2. Open the file to decrypt
3. Read the file one line at a time (i.e., for line in file)
4. For each line call a `decrypt` function you will write (see notes below). The basic alogrithm for that function is as follows:
    - strip() any extra spaces or newlines from the beginning or end of the line
    - create an empty string called `decrypted_line`
    - now you want to iterate over each CHARACTER in the line. For each:
        - if the character is NOT an alphabetical character, just append the character directly to `decrypted_line`
        - if the character is an alphabetical character:
            - `find` the index of the character in your `key` string (will be between 0 and 25)
            - If the index is 0, that means that the encrypted character translates to an `a`, ao append an 'a' to `decrypted_line`
            - If the index is 1, that means that the encrypted character translates to a `b`, ao append a 'b' to `decrypted_line`
            - Again, hopefully you see the pattern...and again using ord() and chr() you can make this really easy by adding 97 (`ord('a')`) to the index and converting that number to a character using chr() (e.g., 0+97 = 97 ==> chr(97) = 'a')
    - RETURN the `decrypted_line` string
5. Back in the main section, just PRINT the decrypted line (*don't* print it in your `decrypt` function...print the returned value in the main section)

## REQUIRED IMPLEMENTATION NOTES 

1.  You MUST create an `encrypt` function that takes a `string` and the `key` as parameters and returns the encrypted value. As noted, this function should be "called" from your main script, which will make it much cleaner. So the following function MUST be defined in your script and called to encrypt each line:
    ``` 
            def encrypt(string, key): 
                ... ENCRYPT THE STRING USING THE KEY ...
                return encrypted_line
    ``` 

2.  You MUST create a `decrypt` function that takes a `string` and the `key` as parameters and returns the decrypted value. As noted, this function should be "called" from your main script, which will make it much cleaner. So the following function MUST be defined in your script and called to encrypt each line:
    ``` 
            def decrypt(string, key): 
                ... DECRYPT THE STRING USING THE KEY ...
                return decrypted_line
    ``` 

3.  When SAVING encrypted files and keys (only happens if doing the "encrypt" path), you should name them by appending `.encrypted` and `.key` to the original file name. For example, if the name of the file to encrypt is `bob.txt`, you will use `bob.txt.encrypted` and `bob.txt.key` as the file names for steps 7 and 8 when encrypting.

4.  You MUST edit `testmsg.txt` and add a line with __your__ name in it; perhaps something like "Paul York was here". __JUST edit and save the file using VS code or another text editor. No Python code required here.__

5.  You MINIMALLY MUST run your script as follows:
    ```
    python crypto_master.py testmsg.txt
    ```
    This should create `testmsg.txt.encrypted` and `testmsg.txt.key`, which you should include in your final submission.

6.  You SHOULD also run your script as follows:
    ```
    python crypto_master.py testmsg.txt.encrypted testmsg.txt.key
    ```
    This should PRINT out the original text stored in testmsg.txt (only in lowercase if you don't do the optional challenge).

7.  I also recommend that you create other test files to encrypt and decrypt, though this is up to you.

When done, be sure to test the heck out of this and then submit the __project1 directory__ in Mimir.

## OPTIONAL CHALLENGES 

1.  By default, the algorithm above lowercases everything. See if you can make it so that you preserve the original case. It's not that much harder, actually...just need to know whether to add/subtract 65 or 97 based on the case of each character.

## HINTS 

1.  The "arguments" to a python program are in the sys.argv list. The first element in the list is the name of the script and the rest are any additional words (arguments) added to the end. [See here for explanation](https://www.tutorialspoint.com/python/python_command_line_arguments.htm).

    I'm asking you to accept 1 or 2 arguments. I.e., calling:
    ```
    python crypto_master.py testmsg.txt
    ```
    is passing in ONE argument ('testmsg.txt'). Calling:
    ```
    python crypto_master.py testmsg.txt.encrypted testmsg.txt.key
    ```
    is passing in TWO arguments ('testmsg.txt.encrypted' and 'testmst.txt.key').

    But since the first argument is always the name of the script, then there's always one "extra" argument in the sys.argv list. SO, if the len of sys.argv isn't 2 or 3 there's an error. If it's 2, then hopefully it's a request to ENCRYPT. And if 3, it's a request to DECRYPT. Working code might thus include the following:
    ```
    import sys

    if len(sys.argv) == 2:
        message_filename = sys.argv[1]
        # ENCRYPT STUFF HERE
    elif len(sys.argv) == 3:
        message_filename = sys.argv[1]
        key_filename = sys.argv[2]
        # DECRYPT STUFF HERE
    else:
        # THIS IS AN ERROR
    ```

2.  Generating a random `key` string basically involves shuffling the alphabet. This would be hard, but of course Python makes it pretty easy if you use the `random` package to generate a full "sample" of an alphabet string. Assuming random has been imported, consider the following code:
    ```
    alphabet = 'abcdefghijklmnopqrstuvwxyz'
    key_list = random.sample(alphabet, len(alphabet))
    key = ''.join(key_list)
    ```
    The above takes the full alphabet, creates a LIST of 26 unique letters (i.e., all of them) drawn randomly from that alphabet string, and then joins them back together into a single string.
