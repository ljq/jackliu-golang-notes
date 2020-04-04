# goroutine

Channel is actually a circular queue. The queue space applies for space after this channel structure.
dataqsiz-> data queue size Queue size. elemsize The size of the element.
Lock is used to ensure thread (coroutine) safety. Recvq and sendq are used to save the corresponding blocking queue.

`` `
struct Hchan
{
 Uintgo qcount; // total number of data in queue q
 Uintgo dataqsiz; // data size of circular queue q
 Uint16 elemsize;
 Closed; closed
 Uint8 elemalign;
 Alg * elemalg; // interface for element type
 Uintgo sendx; s s // send index
 Uintgorecvx; // receive index
 WaitQ: Recvq; // Waiting queue blocked by recv
 WaitQ; sendq; // waiting queue blocked by send
 Lock;
}
`` `

### Blocking considerations
General situation of deadlock:
* There is no inflow out;
* Inflow without outflow

Matters needing attention: special circumstances that will not block

`` `
func main () {
    ch: = make (chan int)
    go func () {
       ch <-1
    } ()
}
`` `
Reason: The main function finishes execution before the goroutine, so no data flows into the main goroutine and no blocking error is sent.



### goroutine usage