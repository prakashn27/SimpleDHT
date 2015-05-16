# SimpleDHT
Designed a simple DHT based on Chord.

## Steps followed:
### Step 1: Writing the Content Provider
First of all, your app should have a content provider. This content provider should implement all DHT functionalities. For example, it should create server and client threads (if this is what you decide to implement), open sockets, and respond to incoming requests; it should also implement a simplified version of the Chord routing protocol; lastly, it should also handle node joins. The following are the requirements for your content provider:
We will test your app with any number of instances up to 5 instances.
The content provider should implement all DHT functionalities. This includes all communication as well as mechanisms to handle insert/query requests and node joins.
Each content provider instance should have a node id derived from its emulator port. This node id should be obtained by applying the above hash function (i.e., genHash()) to the emulator port. For example, the node id of the content provider instance running on emulator-5554 should be, node_id = genHash(“5554”). This is necessary to find the correct position of each node in the Chord ring.
Your content provider should implement insert(), query(), and delete(). The basic interface definition is the same as the previous assignment, which allows a client app to insert arbitrary <”key”, “value”> pairs where both the key and the value are strings.
For delete(URI uri, String selection, String[] selectionArgs), you only need to use use the first two parameters, uri & selection.  This is similar to what you need to do with query().
However, please keep in mind that this “key” should be hashed by the above genHash() before getting inserted to your DHT in order to find the correct position in the Chord ring.
For your query() and delete(), you need to recognize two special strings for the selection parameter.
If “*” (including quotes, not just the single character *, i.e., “\”*\”” should be the string in your code) is given as the selection parameter to query(), then you need to return all <key, value> pairs stored in your entire DHT.
Similarly, if “*” is given as the selection parameter to delete(), then you need to delete all <key, value> pairs stored in your entire DHT.
If “@” (including quotes, not just a single character @, i.e., “\”@\”” should be the string in your code) is given as the selection parameter to query() on an AVD, then you need to return all <key, value> pairs stored in your local partition of the node, i.e., all <key, value> pairs stored locally in the AVD on which you run query().
Similarly, if “@” is given as the selection parameter to delete() on an AVD, then you need to delete all <key, value> pairs stored in your local partition of the node, i.e., all <key, value> pairs stored locally in the AVD on which you run delete().
An app that uses your content provider can give arbitrary <key, value> pairs, e.g., <”I want to”, “store this”>; then your content provider should hash the key via genHash(), e.g., genHash(“I want to”), get the correct position in the Chord ring based on the hash value, and store <”I want to”, “store this”> in the appropriate node.
Your content provider should implement ring-based routing. Following the design of Chord, your content provider should maintain predecessor and successor pointers and forward each request to its successor until the request arrives at the correct node. Once the correct node receives the request, it should process it and return the result (directly or recursively) to the original content provider instance that first received the request.
Your content provider do not need to maintain finger tables and implement finger-based routing. This is not required.
As with the previous assignment, we will fix all the port numbers (see below). This means that you can use the port numbers (11108, 11112, 11116, 11120, & 11124) as your successor and predecessor pointers.
Your content provider should handle new node joins. For this, you need to have the first emulator instance (i.e., emulator-5554) receive all new node join requests. Your implementation should not choose a random node to do that. Upon completing a new node join request, affected nodes should have updated their predecessor and successor pointers correctly.
Your content provider do not need to handle concurrent node joins. You can assume that a node join will only happen once the system completely processes the previous join.
Your content provider do not need to handle insert/query requests while a node is joining. You can assume that insert/query requests will be issued only with a stable system.
Your content provider do not need to handle node leaves/failures. This is not required.
We have fixed the ports & sockets.
Your app should open one server socket that listens on 10000.
You need to use run_avd.py and set_redir.py to set up the testing environment.
The grading will use 5 AVDs. The redirection ports are 11108, 11112, 11116, 11120, and 11124.
You should just hard-code the above 5 ports and use them to set up connections.
Please use the code snippet provided in PA1 on how to determine your local AVD.
emulator-5554: “5554”
emulator-5556: “5556”
emulator-5558: “5558”
emulator-5560: “5560”
emulator-5562: “5562”
Your content provider’s URI should be: “content://edu.buffalo.cse.cse486586.simpledht.provider”, which means that any app should be able to access your content provider using that URI. Your content provider does not need to match/support any other URI pattern.
As with the previous assignment, Your provider should have two columns.
The first column should be named as “key” (an all lowercase string without the quotation marks). This column is used to store all keys.
The second column should be named as “value” (an all lowercase string without the quotation marks). This column is used to store all values.
All keys and values that your provider stores should use the string data type.
Note that your content provider should only store the <key, value> pairs local to its own partition.
Any app (not just your app) should be able to access (read and write) your content provider. As with the previous assignment, please do not include any permission to access your content provider.

### Step 2: Writing the Main Activity
The template has an activity used for your own testing and debugging. It has three buttons, one button that displays “Test”, one button that displays “LDump” and another button that displays “GDump.” As with the previous assignment, “Test” button is already implemented (it’s the same as “PTest” from the last assignment). You can implement the other two buttons to further test your DHT.
LDump
When touched, this button should dump and display all the <key, value> pairs stored in your local partition of the node.
This means that this button can give “@” as the selection parameter to query().
GDump
When touched, this button should dump and display all the <key, value> pairs stored in your whole DHT. Thus, LDump button is for local dump, and this button (GDump) is for global dump of the entire <key, value> pairs.
This means that this button can give “*” as the selection parameter to query().

