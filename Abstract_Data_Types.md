# Abstract Data Type
* An abstract data type (ADT) is a mathematical model for data types, based on its behavior as observed by the user of data.
* This is very different from a data structure, which is a concrete representation of data from the point of view of an implementer.
* Most mainstream computer languages don't directly support formally specifying ADTs, but they correspond to certain aspects of implementing ADTs, like abstract types, opaque data types, protocols, and design by contract.
* EX: In modular programming, the module declares procedures that correspond to ADT operations, and this information hiding strategy allows the implementation of the module to be changed without disturbing the client programs, but this is only an informat ADT.

## Definition
* An ADT consists of a domain, a collection of operations, and a set of constraints the operations must satisfy.
* The interface of an ADT typically regers only to the domain and operations, and some constraints on operations (like pre- and post-conditions) but not all of them (like relations between operators, which are considered behavior).
* There are two major types of formal specifications for AD Structure behavior...
  1. Axiomatic Semantics-each operation is modelled as a mathematical function with no side effects. The same operations applied to the same arguments will always yield the same results.
  2. Operational Semantics-an ADS is conceived as an entity that's mutable, meaning there is a notion of time and the ADT may be in different states at different times. Operations change the state of the ADT over time, so the same operation on same entities may have different effects if executed at different times.
 
## Auxilary Operations
* create() yields a new instance of the ADT
* compare(s,t) tests whether two instances' states are equivalent in some sense
* hash(s) computes a hash function from the instance's state
* print(s) or show(s) produces a human readable representation of s' state
* initialize(s) prepares a new instance of s or resets it to some "initial state"
* copy(s,t) puts s in a state equivalent to that of t
* clone(t) that perofrms s<-(create), copy(s,t) and returns s
* free(s) or destroy(s) reclaims memory used by s, and can be used to analyze the storage used by an algorithm that uses the ADT.

## Restricted Types
* The definition of an ADT usually restricts the stored value(s) for its instances to members of a specific set X called the range of those variables (EX: integers)

## Aliasing
* A common style of defining ADTs writes operations as if one instance exists during the execution of the algorithm and all operations are applied to that instance (EX: push(x) may operate on the only existing stack)
* You can also exclude partial aliasing with other instances (stating that changing a field of one record variable does not affect any other records)

# Examples
## Abstract Variable
* The simplest non-trivial ADT, with semantics of an imperative variable. It has two operations, fetch(v), which returns the current value in location v, and store(v,x) stores the value x in location V.
* Names are always distinct; storing a value into variable U has no effect on the state of a distinct variable v.
* Fetching before storing can be disallowed, defined to have a certain result, or left unspecified.

## Abstract Stack
* Last-in-first-out structure. It has three key operations..
  1. push-inserts a data item onto the stack
  2. pop-removes a data item from the stack
  3. peek/top-accesses a data item on top of the stack without removal
  * A complete abstract stack definition includes also a Boolean-valued function empty(s) and create() that returns an initial stack instance.

## Boom Hierarchy
* Refers to the binary tree, list, bag, and set abstract data types. All can be declared by three operations...
  1. null-constructs empty container
  2. single-constructs a container from a single element
  3. append-combines two containers of the same type
* The complete specification of the four data types can then be given by adding the following rules over these operations...
  1. null is the left and right neutral for a tree
  2. lists add that append is associative
  3. bags add commutativity (append(B,A)=append(A,B))
  4. sets are also idempotent
* Access to the data can be specified by pattern-matching over the three operators, EX: A member function for these containers by:
    * member(X, single(Y))=eq(X,Y)
    * member(X, null)=false
    * member(X, append(A,B))=or(member(X,A), member(X,B))

## Common ADTs
* Collection
* Container
* List
* String
* Set
* Graph
* Tree
* Stack
* Queue
* Double-ended priority queue
