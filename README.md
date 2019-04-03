# Custom-console

Created two versions of a custom command line interface suing JavaScript/jQuery

Version 1 [In-Memory Database] -
SETUP:
•	Used a web command line interface, link to the library - https://github.com/1j01/simple-console.
•	Included files simple-console.css and simple-console.js and created an instance of the console, new SimpleConsole(options).
COMMANDS:
A Web Storage object called localStorage was used for storing data within the browser.
To perform any of the operations, the input was first converted into an array.
•	SET: If the first element of the input, input[0] is SET, created a key-value pair in the localStorage with input[1] being the key and input[2] being the value.
•	GET: Since localStorage stores data in string format, the obtained value from localStorage for the given variable (input[1]) was JSON parsed. If the value doesn’t exist, NULL is printed out.
•	UNSET: Value of the corresponding key i.e, input[1] is set to null.
•	COUNT: Looped through the entire localStorage object, if a key’s value is equal to input[1], then the counter was incremented and the corresponding count was returned.
•	END: When this command is entered, the divs that provide the space for entering commands are not displayed anymore.

Version 2 [In-Memory database with Transactions] -
COMMANDS:
BEGIN: 
•	The function checks the number of BEGINS in localStorage indicating the number of transactional blocks. 
•	This is tracked by beginstatus and statusnumber. Beginstatus is incremented every time a BEGIN is entered, updating the statusnumber which is used throughout the code.
•	An array key, in the format arr followed by statusnumber (ex. arr1) is created in localStorage every time a BEGIN is entered. Any transaction following this BEGIN will have their corresponding values stored in the array.
•	If this command is entered for the first time, status key is created in localStorage and initialised to 1. Status indicates the number of BEGINs in localStoage.
•	Every time BEGIN is entered, the function checks to see if there are any previous blocks. If so, values from the most recent previous block are copied to the current block.
•	If it is the first BEGIN, localStorage is checked for values (performed by outBlock function). Any value outside the block indicates committed values and these are copied over to the first BEGIN block.
ROLLBACK:
•	If this command is entered, the most recent transactional block/array is removed from localStorage. This is done by tracking the last statusnumber, decrementing statusnumber and beginstatus by one and updating status in localStorage.
•	If the values are committed or permanently stored in localStorage (i.e, outside transactional blocks) and this is indicated by the key commit which will be set to 1 in such cases, then it would result in an INVALID ROLLBACK.
COMMIT:
•	COMMIT takes the most recent block, i.e, recent array’s values and commits it to a new key-value pair in localStorage outside of these arrays. All arrays (transactional blocks) are deleted including status key and a new key called commit is added to localStorage which is set to 1.
SET:
•	If this command is inside a BEGIN, then the variable and its value are added to the most recent array of transaction. Otherwise they are added as a new key-value pair in localStorage indicating that it is a permanent/committed transaction
GET:
•	This grabs the values from the most recent transaction array or from localStorage if there are no transaction arrays open. NULL is printed out if no value exists.
UNSET:
•	Checks the last transaction to set the variable to null or checks localStorage (if no open transaction) for the variable to do the same.
COUNT:
•	If input[1] is present in the most recent transaction array, counter is incremented. If the counter is not 0, value is returned otherwise, NULL is printed out.
END:
•	When this command is entered, the divs that provide the space for entering commands are not displayed anymore.
