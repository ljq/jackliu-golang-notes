# Lock mechanism usage

### Precautions
* The smaller the granularity of the lock, the better, and release the lock as soon as possible after locking.
* Release the lock in time when the current logic is executed to prevent the lock from being delayed by other logic.
* Try to**do not use defer**to release locks to avoid excessively large lock granularity or too long holding time when logic is too much, and locks cannot be released in time (see defer operating mechanism), although defer applies to resource release Scene, but not suitable for holding and releasing the lock**.
* Try to**do not nest read locks**, otherwise it will cause read locks and write locks to wait for each other to send signals in some scenarios, resulting in deadlocks or dead loops.