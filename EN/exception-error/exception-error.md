# Excepton & Error

### Golang's design philosophy for handling exceptions

* If the function may have an exception, it should be treated as the return value, and if there are no exceptions, it should return nil;
* **Guard statement**: When abnormal functions may occur, it is necessary to actively check the if statement and make a response;

### Custom error or exception

* Create information: ```errors. New ("message")```, it is recommended to use  ```fmt. Errorf ("% v", "message")``` to format the output
* Printing Exception: ```err. Error()```


