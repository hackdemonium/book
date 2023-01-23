## Τι είναι η Ιδιοκτησία;

Η ιδιοκτησία *Ownership*  είναι ένα σύνολο κανόνων που διέπουν τον τρόπο με τον οποίο 
ένα πρόγραμμα Rust διαχειρίζεται τη μνήμη. Όλα τα προγράμματα πρέπει να διαχειρίζουν 
τον τρόπο με τον οποίο χρησιμοποιούν τη μνήμη ενός υπολογιστή κατά την εκτέλεση. 
Ορισμένες γλώσσες διαθέτουν συλλογή σκουπιδιών που αναζητά τακτικά μνήμη που δεν χρησιμοποιείται 
πλέον καθώς εκτελείται το πρόγραμμα. Σε άλλες γλώσσες, ο προγραμματιστής πρέπει να εκχωρήσει 
και να ελευθερώσει ρητά τη μνήμη. Η Rust χρησιμοποιεί μια τρίτη προσέγγιση: η διαχείριση 
της μνήμης γίνεται μέσω ενός συστήματος ιδιοκτησίας, με ένα σύνολο κανόνων που ελέγχει ο μεταγλωττιστής. 
Εάν κάποιος από τους κανόνες παραβιαστεί, το πρόγραμμα δεν θα μεταγλωττιστεί. Καμία από τις δυνατότητες 
ιδιοκτησίας δεν θα επιβραδύνει το πρόγραμμά σας ενώ εκτελείται.

Επειδή η ιδιοκτησία είναι μια νέα έννοια για πολλούς προγραμματιστές, χρειάζεται λίγος χρόνος
για να τη συνηθίσουν. Τα καλά νέα είναι ότι όσο πιο έμπειροι γίνεστε με τη Rust και τους κανόνες του 
συστήματος ιδιοκτησίας, τόσο πιο εύκολη είναι η φυσική ανάπτυξη κώδικα που είναι ασφαλής και αποτελεσματικός. Κρατήστε το!

Όταν κατανοήσετε την ιδιοκτησία, θα έχετε μια σταθερή βάση για να κατανοήσετε τα χαρακτηριστικά που κάνουν τη Rust μοναδική. 
Σε αυτό το κεφάλαιο, θα μάθετε την ιδιοκτησία δουλεύοντας σε μερικά παραδείγματα που εστιάζονται σε μια πολύ κοινή δομή δεδομένων: τις συμβολοσειρές.


> ### Η Στοίβα και ο Σωρός
>
> Οι περισσότερες γλώσσες προγραμματισμού δεν σας ζητούν να σκέφτεστε τακτικά τη Στοίβα και το Σωρό.
> Όμως στις γλώσσες προγραμματισμού συστήματος όπως η Rust κάθε φορά που μια τιμή είναι στη στοίβα
> ή στο σωρό, επηρεάζεται την συμπεριφορά της γλώσσας και το γιατί πρέπει να πάρουμε συγκεκριμένες αποφάσεις.
> Μέρη της σχέσης ιδιοκτησίας με τη στοίβα και το σωρό, θα συζητηθούν αργότερα στο κεφάλαιο, οπότε εδώ προετοιμάσαμε
> μια σύντομη περιγραφή.
> 
>Τόσο η στοίβα όσο και ο σωρός είναι τμήματα της μνήμης που είναι διαθέσιμα για χρήση από τον κώδικα κατά το χρόνο εκτέλεσης.
>Η στοίβα αποθηκεύει τις τιμές με τη σειρά που τις λαμβάνει και τις αφαιρεί με την αντίστροφη σειρά.
>Αυτό αναφέρεται ως *last in, first out*. Ας σκεφτούμε μια στοίβα από πιάτα: όταν προσθέτεις περισσότερα πιάτα, τα τοποθετείς
>στην κορυφή της στοίβας και όταν χρειάζεσαι ένα πιάτο, παίρνεις ένα από την κορυφή. Η πρόσθεση και η αφαίρεση πιάτων από τη μέση
>δεν θα δούλευε σωστα! Η πρόσθεση δεδομένων ονομάζεται *pushing onto the stack* και η αφαίρεση *popping off the stack*. Αντίθετα τα δεδομένα 
>για τα οποία το μέγεθός τους είναι άγνωστο κατά τον χρόνο εκτέλεσης θα πρέπει να αποθηκεύονται στο Σωρό.
>
> Ο σωρός είναι λιγότερο οργανωμένος: όταν τοποθετείς δεδομένα στο σωρό, ζητάς μία συγκεκριμένη ποσότητα αποθηκευτικού χώρου.
> Ο εκχωρητής μνήμης βρίσκει ένα άδειο σημείο στο σωρό, το οποίο να είναι αρκετα μεγάλο και το μαρκάρει προς χρήση, ενω επιστρέφει
> έναν δείκτη *pointer* ο οποίος είναι η διεύθυνση αυτού του σημείου. Η διαδικασία αυτή ονομάζεται *allocating on the
> heap* ή πιο σύντομα απλά *allocating* (το σπρωξιμο τιμών στο σωρό δεν θεωρείται κατανομή). Επειδή ο δείκτης στο σωρό είναι γνωστός, σταθερού
> μεγέθους, μπορείτε να τον αποθηκεύσετε στη στοίβα, όμως όταν θέλετε τα πραγματικά δεδομένα, πρέπει να ακολουθήσετε τον δείκτη.
> Σκεφτείτε ότι θέλετε να καθίσετε σε ένα εστιατόριο. Αρχικά μπαίνετε και δηλώνετε τον αριθμό των ατόμων και ο οικοδεσπότης βρίσκει ένα
> άδειο τραπέζι που θα σας χωράει και σας οδηγεί εκεί. Εάν κάποιος από την παρέα έρθει αργότερα θα ρωτήσει που κάθεστε για να σας βρει.
>
> Η τοποθέτηση στη στοίβα είναι ταχύτερη από την κατανομή στο σωρό, επειδή ο εκχωρητής δεν χρειάζεται ποτέ να αναζητήσει ένα μέρος για να αποθηκεύσει 
> νέα δεδομένα καθώς η τοποθεσία είναι πάντα στην κορυφή της στοίβας. Συγκριτικά, η κατανομή χώρου στο σωρό απαιτεί περισσότερη δουλειά επειδή 
> ο εκχωρητής πρέπει πρώτα να βρει έναν αρκετά μεγάλο χώρο για να κρατήσει τα δεδομένα και στη συνέχεια να εκτελέσει λογιστική για να 
> προετοιμαστεί για την επόμενη κατανομή.
>
> Η πρόσβαση σε δεδομένα στο σωρό είναι πιο αργή από την πρόσβαση σε δεδομένα στη στοίβα, επειδή πρέπει να ακολουθήσετε έναν δείκτη για να φτάσετε εκεί. 
> Οι σύγχρονοι επεξεργαστές είναι πιο γρήγοροι εάν κάνουν λιγότερα άλματα στη μνήμη. Συνεχίζοντας την αναλογία, σκεφτείτε έναν σερβιτόρο στο εστιατόριο 
> που δέχεται παραγγελίες από πολλά τραπέζια. Είναι πιο αποτελεσματικό να λαμβάνει όλες τις παραγγελίες σε ένα τραπέζι πριν προχωρήσει στο επόμενο τραπέζι. 
> Η λήψη μιας παραγγελίας από το τραπέζι Α, στη συνέχεια μιας παραγγελίας από τον τραπέζι Β, μετά μίας από το Α ξανά και μετά μίας από το Β ξανά, 
> θα ήταν πολύ πιο αργή διαδικασία. Με την ίδια λογική, ένας επεξεργαστής μπορεί να κάνει τη δουλειά του καλύτερα εάν εργάζεται σε δεδομένα που είναι 
> κοντά σε άλλα δεδομένα (όπως είναι στη στοίβα) παρά σε πιο μακριά (όπως μπορεί να είναι στο σωρό).
>
> Όταν ο κώδικάς σας καλεί μια συνάρτηση, οι τιμές που μεταβιβάζονται στη συνάρτηση (συμπεριλαμβανομένων, ενδεχομένως, δεικτών σε δεδομένα στο σωρό) 
> και οι τοπικές μεταβλητές της συνάρτησης ωθούνται στη στοίβα. Όταν τελειώσει η λειτουργία, αυτές οι τιμές αφαιρούνται από τη στοίβα.
> 
>Η παρακολούθηση των τμημάτων του κώδικα που χρησιμοποιούν τα δεδομένα στο σωρό, η ελαχιστοποίηση του όγκου των διπλότυπων δεδομένων στο σωρό και 
>ο καθαρισμός των αχρησιμοποίητων δεδομένων στο σωρό, ώστε να μην εξαντληθεί ο χώρος είναι όλα προβλήματα που αντιμετωπίζει η ιδιοκτησία. 
>Μόλις κατανοήσετε την ιδιοκτησία, δεν θα χρειάζεται να σκέφτεστε πολύ συχνά τη στοίβα και το σωρό, αλλά γνωρίζοντας ότι ο κύριος σκοπός της 
>είναι η διαχείριση δεδομένων σωρού μπορεί να σας βοηθήσει να εξηγήσετε γιατί λειτουργεί με τον τρόπο που λειτουργεί.


### Οι κανόνες της Ιδιοκτησίας

Αρχικά ας ρίξουμε μια ματιά στους κανόνες της Ιδιοκτησίας. Κρατήστε τιυς στο μυαλό σας καθώς θα δουλεύουμε τα παραδείγματα
που τους επεξηγούν:

* Κάθε τιμή στη Rust έχει έναν ιδιοκτήτη *owner*.
* Σε κάθε στιγμή μπορεί να υππάρχει μόνο ένας ιδιοκτήτης.
* Όταν ο ιδιοκτήτης βγαίνει εκτός πεδίου δράσης, η τιμή απορρίπτεται.

### Πεδίο δράσης Μεταβλητής

Τώρα που έχουμε ξεπεράσει τη βασική σύνταξη της Rust, δεν θα συμπεριλαμβάνουμε όλον τον `fn main() {` κώδικα στα παραδείγματα
οπότε φροντίστε εάν τα ακολουθείτε, να τοποθετείτε τα παραδείγματα από μόνοι σας μέσα σε μια συνάρτηση `main`. Αυτό θα έχει 
αποτέλεσμα τα παραδείγματά μας να είναι συνοπτικά, επιτρέποντας μας να επικεντρωθούμε σε συγκεκριμένες λεπτομέρειες, παρά σε ένα  
πρότυπο κώδικα.

Ως πρώτο παράδειγμα ιδιοκτησίας, θα δούμε το πεδίο δράσης *scope* ορισμένων μεταβλητών. Το πεδίο δράσης είναι ένα διάστημα εντός 
τουν προγράμματος κατά το οποίο ένα αντικείμενο είναι έκγυρο. Έχουμε την παρακάτω μεταβλητή:

```rust
let s = "hello";
```

Η μεταβλητή `s`  σε μία κυριολεκτική συμβολοσειρά, η τιμή της οποίας έχει κωδικοποιηθεί επακριβώς στο κείμενο του προγράμματός μας.
Η μεταβλητή είναι έγκυρη από το σημείο δήλωσης μέχρι το τέλος του πεδίου δράσης της. Το Listing 4-1  μας δείχνεί ένα πρόγραμμα 
με σχόλια που περιγράφουν που η μεταβλητή `s` θα είναι έγκυρη.

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-01/src/main.rs:here}}
```

<span class="caption">Listing 4-1: Η Μεταβλητή και το πεδίο δράσης κατά το οποίο είναι έγκυρη</span>

Με άλλα λόγια, εδώ υπάρχουν δύο σημαντικά σημεία στον χρόνο: 

* Όταν η `s` εμφανίζεται μέσα *into* στο πεδίο δράσης, είναι έγκυρη.
* Παραμένει έγκυρη εως ότου βγει εκτός *out of* πεδίου δράσης.

Σε αυτό το σημείο, η σχέδη ανάμεσα πεδίων δράσης και πότε οι μεταβλητές είναι έγκυρες, είναι πανομοιότυπη με τις άλλες γλώσσες
προγραμματισμού. Τώρα θα δουλέψουμε με βάση αυτή τη κατανόηση, με την παρουσίαση του τύπου `String`.

### Ο τύπος Συμβολοσειρά `String`

Για να επεξηγήσουμε τους κανόνες της ιδιοκτησίας, χρειαζόμαστε έναν πιο πολύπλοκο τύπο δεδομένων, σε σχέση με αυτούς που καλύψαμε στην ενότητα [“Data Types”][data-types], του Κεφαλαίου 3 <!-- ignore --> . Αυτοί οι τύποι είχαν γνωστό μέγεθος και μπορούσαν να αποθηκευτούν στη στοίβα, και να απορριφθούν από
αυτή όταν είχε τελειώσει το πεδίο δράσης τους, ενώ μπορούσαν γρήγορα και τετριμμένα να αντιγραφούν ώστε να δημιουργηθούν νέα, ανεξάρτητα στιγμιότυπά τους
έαν κάποιο άλλο κωμάτι κώδικα χεριαζόταν την τιμή του σε άλλο πεδίο δράσης. Όμως θέλουμε να εστιάσουμε σε δεδομένα που αποθηκεύονται στο σωρό και
να ανακαλύψουμε πως γνωρίζει η Rust πότε θα καθαρίσει αυτά τα δεδομένα και για το λόγο αυτό ο τύπος `String` είναι εξαιρετικό παράδειγμα.

We’ll concentrate on the parts of `String` that relate to ownership. These
aspects also apply to other complex data types, whether they are provided by
the standard library or created by you. We’ll discuss `String` in more depth in
[Chapter 8][ch8]<!-- ignore -->.

We’ve already seen string literals, where a string value is hardcoded into our
program. String literals are convenient, but they aren’t suitable for every
situation in which we may want to use text. One reason is that they’re
immutable. Another is that not every string value can be known when we write
our code: for example, what if we want to take user input and store it? For
these situations, Rust has a second string type, `String`. This type manages
data allocated on the heap and as such is able to store an amount of text that
is unknown to us at compile time. You can create a `String` from a string
literal using the `from` function, like so:

```rust
let s = String::from("hello");
```

The double colon `::` operator allows us to namespace this particular `from`
function under the `String` type rather than using some sort of name like
`string_from`. We’ll discuss this syntax more in the [“Method
Syntax”][method-syntax]<!-- ignore --> section of Chapter 5, and when we talk
about namespacing with modules in [“Paths for Referring to an Item in the
Module Tree”][paths-module-tree]<!-- ignore --> in Chapter 7.

This kind of string *can* be mutated:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-01-can-mutate-string/src/main.rs:here}}
```

So, what’s the difference here? Why can `String` be mutated but literals
cannot? The difference is in how these two types deal with memory.

### Memory and Allocation

In the case of a string literal, we know the contents at compile time, so the
text is hardcoded directly into the final executable. This is why string
literals are fast and efficient. But these properties only come from the string
literal’s immutability. Unfortunately, we can’t put a blob of memory into the
binary for each piece of text whose size is unknown at compile time and whose
size might change while running the program.

With the `String` type, in order to support a mutable, growable piece of text,
we need to allocate an amount of memory on the heap, unknown at compile time,
to hold the contents. This means:

* The memory must be requested from the memory allocator at runtime.
* We need a way of returning this memory to the allocator when we’re done with
  our `String`.

That first part is done by us: when we call `String::from`, its implementation
requests the memory it needs. This is pretty much universal in programming
languages.

However, the second part is different. In languages with a *garbage collector
(GC)*, the GC keeps track of and cleans up memory that isn’t being used
anymore, and we don’t need to think about it. In most languages without a GC,
it’s our responsibility to identify when memory is no longer being used and to
call code to explicitly free it, just as we did to request it. Doing this
correctly has historically been a difficult programming problem. If we forget,
we’ll waste memory. If we do it too early, we’ll have an invalid variable. If
we do it twice, that’s a bug too. We need to pair exactly one `allocate` with
exactly one `free`.

Rust takes a different path: the memory is automatically returned once the
variable that owns it goes out of scope. Here’s a version of our scope example
from Listing 4-1 using a `String` instead of a string literal:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-02-string-scope/src/main.rs:here}}
```

There is a natural point at which we can return the memory our `String` needs
to the allocator: when `s` goes out of scope. When a variable goes out of
scope, Rust calls a special function for us. This function is called
[`drop`][drop]<!-- ignore -->, and it’s where the author of `String` can put
the code to return the memory. Rust calls `drop` automatically at the closing
curly bracket.

> Note: In C++, this pattern of deallocating resources at the end of an item’s
> lifetime is sometimes called *Resource Acquisition Is Initialization (RAII)*.
> The `drop` function in Rust will be familiar to you if you’ve used RAII
> patterns.

This pattern has a profound impact on the way Rust code is written. It may seem
simple right now, but the behavior of code can be unexpected in more
complicated situations when we want to have multiple variables use the data
we’ve allocated on the heap. Let’s explore some of those situations now.

<!-- Old heading. Do not remove or links may break. -->
<a id="ways-variables-and-data-interact-move"></a>

#### Variables and Data Interacting with Move

Multiple variables can interact with the same data in different ways in Rust.
Let’s look at an example using an integer in Listing 4-2.

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-02/src/main.rs:here}}
```

<span class="caption">Listing 4-2: Assigning the integer value of variable `x`
to `y`</span>

We can probably guess what this is doing: “bind the value `5` to `x`; then make
a copy of the value in `x` and bind it to `y`.” We now have two variables, `x`
and `y`, and both equal `5`. This is indeed what is happening, because integers
are simple values with a known, fixed size, and these two `5` values are pushed
onto the stack.

Now let’s look at the `String` version:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-03-string-move/src/main.rs:here}}
```

This looks very similar, so we might assume that the way it works would be the
same: that is, the second line would make a copy of the value in `s1` and bind
it to `s2`. But this isn’t quite what happens.

Take a look at Figure 4-1 to see what is happening to `String` under the
covers. A `String` is made up of three parts, shown on the left: a pointer to
the memory that holds the contents of the string, a length, and a capacity.
This group of data is stored on the stack. On the right is the memory on the
heap that holds the contents.

<img alt="Two tables: the first table contains the representation of s1 on the
stack, consisting of its length (5), capacity (5), and a pointer to the first
value in the second table. The second table contains the representation of the
string data on the heap, byte by byte." src="img/trpl04-01.svg" class="center"
style="width: 50%;" />

<span class="caption">Figure 4-1: Representation in memory of a `String`
holding the value `"hello"` bound to `s1`</span>

The length is how much memory, in bytes, the contents of the `String` are
currently using. The capacity is the total amount of memory, in bytes, that the
`String` has received from the allocator. The difference between length and
capacity matters, but not in this context, so for now, it’s fine to ignore the
capacity.

When we assign `s1` to `s2`, the `String` data is copied, meaning we copy the
pointer, the length, and the capacity that are on the stack. We do not copy the
data on the heap that the pointer refers to. In other words, the data
representation in memory looks like Figure 4-2.

<img alt="Three tables: tables s1 and s2 representing those strings on the
stack, respectively, and both pointing to the same string data on the heap."
src="img/trpl04-02.svg" class="center" style="width: 50%;" />

<span class="caption">Figure 4-2: Representation in memory of the variable `s2`
that has a copy of the pointer, length, and capacity of `s1`</span>

The representation does *not* look like Figure 4-3, which is what memory would
look like if Rust instead copied the heap data as well. If Rust did this, the
operation `s2 = s1` could be very expensive in terms of runtime performance if
the data on the heap were large.

<img alt="Four tables: two tables representing the stack data for s1 and s2,
and each points to its own copy of string data on the heap."
src="img/trpl04-03.svg" class="center" style="width: 50%;" />

<span class="caption">Figure 4-3: Another possibility for what `s2 = s1` might
do if Rust copied the heap data as well</span>

Earlier, we said that when a variable goes out of scope, Rust automatically
calls the `drop` function and cleans up the heap memory for that variable. But
Figure 4-2 shows both data pointers pointing to the same location. This is a
problem: when `s2` and `s1` go out of scope, they will both try to free the
same memory. This is known as a *double free* error and is one of the memory
safety bugs we mentioned previously. Freeing memory twice can lead to memory
corruption, which can potentially lead to security vulnerabilities.

To ensure memory safety, after the line `let s2 = s1;`, Rust considers `s1` as
no longer valid. Therefore, Rust doesn’t need to free anything when `s1` goes
out of scope. Check out what happens when you try to use `s1` after `s2` is
created; it won’t work:

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-04-cant-use-after-move/src/main.rs:here}}
```

You’ll get an error like this because Rust prevents you from using the
invalidated reference:

```console
{{#include ../listings/ch04-understanding-ownership/no-listing-04-cant-use-after-move/output.txt}}
```

If you’ve heard the terms *shallow copy* and *deep copy* while working with
other languages, the concept of copying the pointer, length, and capacity
without copying the data probably sounds like making a shallow copy. But
because Rust also invalidates the first variable, instead of being called a
shallow copy, it’s known as a *move*. In this example, we would say that `s1`
was *moved* into `s2`. So, what actually happens is shown in Figure 4-4.

<img alt="Three tables: tables s1 and s2 representing those strings on the
stack, respectively, and both pointing to the same string data on the heap.
Table s1 is grayed out be-cause s1 is no longer valid; only s2 can be used to
access the heap data." src="img/trpl04-04.svg" class="center" style="width:
50%;" />

<span class="caption">Figure 4-4: Representation in memory after `s1` has been
invalidated</span>

That solves our problem! With only `s2` valid, when it goes out of scope it
alone will free the memory, and we’re done.

In addition, there’s a design choice that’s implied by this: Rust will never
automatically create “deep” copies of your data. Therefore, any *automatic*
copying can be assumed to be inexpensive in terms of runtime performance.

<!-- Old heading. Do not remove or links may break. -->
<a id="ways-variables-and-data-interact-clone"></a>

#### Variables and Data Interacting with Clone

If we *do* want to deeply copy the heap data of the `String`, not just the
stack data, we can use a common method called `clone`. We’ll discuss method
syntax in Chapter 5, but because methods are a common feature in many
programming languages, you’ve probably seen them before.

Here’s an example of the `clone` method in action:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-05-clone/src/main.rs:here}}
```

This works just fine and explicitly produces the behavior shown in Figure 4-3,
where the heap data *does* get copied.

When you see a call to `clone`, you know that some arbitrary code is being
executed and that code may be expensive. It’s a visual indicator that something
different is going on.

#### Stack-Only Data: Copy

There’s another wrinkle we haven’t talked about yet. This code using
integers—part of which was shown in Listing 4-2—works and is valid:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-06-copy/src/main.rs:here}}
```

But this code seems to contradict what we just learned: we don’t have a call to
`clone`, but `x` is still valid and wasn’t moved into `y`.

The reason is that types such as integers that have a known size at compile
time are stored entirely on the stack, so copies of the actual values are quick
to make. That means there’s no reason we would want to prevent `x` from being
valid after we create the variable `y`. In other words, there’s no difference
between deep and shallow copying here, so calling `clone` wouldn’t do anything
different from the usual shallow copying, and we can leave it out.

Rust has a special annotation called the `Copy` trait that we can place on
types that are stored on the stack, as integers are (we’ll talk more about
traits in [Chapter 10][traits]<!-- ignore -->). If a type implements the `Copy`
trait, variables that use it do not move, but rather are trivially copied,
making them still valid after assignment to another variable.

Rust won’t let us annotate a type with `Copy` if the type, or any of its parts,
has implemented the `Drop` trait. If the type needs something special to happen
when the value goes out of scope and we add the `Copy` annotation to that type,
we’ll get a compile-time error. To learn about how to add the `Copy` annotation
to your type to implement the trait, see [“Derivable
Traits”][derivable-traits]<!-- ignore --> in Appendix C.

So, what types implement the `Copy` trait? You can check the documentation for
the given type to be sure, but as a general rule, any group of simple scalar
values can implement `Copy`, and nothing that requires allocation or is some
form of resource can implement `Copy`. Here are some of the types that
implement `Copy`:

* All the integer types, such as `u32`.
* The Boolean type, `bool`, with values `true` and `false`.
* All the floating-point types, such as `f64`.
* The character type, `char`.
* Tuples, if they only contain types that also implement `Copy`. For example,
  `(i32, i32)` implements `Copy`, but `(i32, String)` does not.

### Ownership and Functions

The mechanics of passing a value to a function are similar to those when
assigning a value to a variable. Passing a variable to a function will move or
copy, just as assignment does. Listing 4-3 has an example with some annotations
showing where variables go into and out of scope.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-03/src/main.rs}}
```

<span class="caption">Listing 4-3: Functions with ownership and scope
annotated</span>

If we tried to use `s` after the call to `takes_ownership`, Rust would throw a
compile-time error. These static checks protect us from mistakes. Try adding
code to `main` that uses `s` and `x` to see where you can use them and where
the ownership rules prevent you from doing so.

### Return Values and Scope

Returning values can also transfer ownership. Listing 4-4 shows an example of a
function that returns some value, with similar annotations as those in Listing
4-3.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-04/src/main.rs}}
```

<span class="caption">Listing 4-4: Transferring ownership of return
values</span>

The ownership of a variable follows the same pattern every time: assigning a
value to another variable moves it. When a variable that includes data on the
heap goes out of scope, the value will be cleaned up by `drop` unless ownership
of the data has been moved to another variable.

While this works, taking ownership and then returning ownership with every
function is a bit tedious. What if we want to let a function use a value but
not take ownership? It’s quite annoying that anything we pass in also needs to
be passed back if we want to use it again, in addition to any data resulting
from the body of the function that we might want to return as well.

Rust does let us return multiple values using a tuple, as shown in Listing 4-5.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-05/src/main.rs}}
```

<span class="caption">Listing 4-5: Returning ownership of parameters</span>

But this is too much ceremony and a lot of work for a concept that should be
common. Luckily for us, Rust has a feature for using a value without
transferring ownership, called *references*.

[data-types]: ch03-02-data-types.html#data-types
[ch8]: ch08-02-strings.html
[traits]: ch10-02-traits.html
[derivable-traits]: appendix-03-derivable-traits.html
[method-syntax]: ch05-03-method-syntax.html#method-syntax
[paths-module-tree]: ch07-03-paths-for-referring-to-an-item-in-the-module-tree.html
[drop]: ../std/ops/trait.Drop.html#tymethod.drop
