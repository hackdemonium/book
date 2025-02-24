
## Mεταβλητές και Μεταβλητότητα

Όπως αναφέρθηκε στην ενότητα [“Storing Values with Variables”][storing-values-with-variables]<!-- ignore -->, από προεπιλογή, οι μεταβλητές είναι αμετάβλητες. Είναι μια από τις πολλές ωθήσεις που σας δίνει η Rust, ώστε να γράψετε τον κώδικά σας με τρόπο που να εκμεταλλεύεται την ασφάλεια και τον εύκολο συγχρονισμό που προσφέρει η Rust. Ωστόσο, εξακολουθείτε να έχετε την επιλογή να κάνετε τις μεταβλητές σας ευμετάβλητες. Ας εξερευνήσουμε πώς και γιατί η Rust σας ενθαρρύνει να προτιμάτε την αμετάβλητότητα και γιατί μερικές φορές μπορεί να θέλετε να κάνετε εξαίρεση.

Όταν μια μεταβλητή είναι αμετάβλητη, αν μια τιμή δεσμευτεί σε ένα όνομα, δεν μπορείτε να αλλάξετε αυτήν την τιμή. Για να το δείτε αυτό, δημιουργήστε ένα νέο προτζεκτ που ονομάζεται *variables*  στον φάκελο των προγραμμάτων σας χρησιμοποιώντας την εντολή `cargo new variables`.

Στη συνέχεια, στον νέο σας φάκελο *variables*, ανοίξτε το *src/main.rs* και αντικαταστήστε τον κώδικά του με τον ακόλουθο κώδικα, ο οποίος δεν θα μεταγλωττιστεί ακόμα:

<span class="filename">Όνομα αρχείου: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-01-variables-are-immutable/src/main.rs}}
```

Αποθηκεύστε και εκτελέστε το πρόγραμμα χρησιμοποιώντας την εντολή `cargo run`. Θα πρέπει να λάβετε ένα μήνυμα σφάλματος σχετικά με ένα σφάλμα μη μεταβλητότητας, όπως φαίνεται σε αυτή την έξοδο:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-01-variables-are-immutable/output.txt}}
```

Αυτό το παράδειγμα δείχνει πώς ο μεταγλωττιστής σάς βοηθά να βρείτε σφάλματα στα προγράμματά σας. Τα σφάλματα μεταγλωττιστή μπορεί να είναι ενοχλητικά, αλλά στην πραγματικότητα το μόνο που σημαίνουν είναι ότι το πρόγραμμά σας δεν κάνει ακόμα με ασφάλεια αυτό που θέλετε να κάνει. δεν σημαίνουν ότι δεν είσαι καλός προγραμματιστής! Οι έμπειροι Rustaceans εξακολουθούν να λαμβάνουν σφάλματα μεταγλωττιστή.


Λάβατε το μήνυμα σφάλματος`` cannot assign twice to immutable variable `x` `` επειδή προσπαθήσατε να εκχωρήσετε μια δεύτερη τιμή στην αμετάβλητη μεταβλητή `x`.

Είναι σημαντικό να λαμβάνουμε σφάλματα χρόνου μεταγλώττισης όταν προσπαθούμε να αλλάξουμε μια τιμή που έχει οριστεί ως αμετάβλητη, επειδή αυτή ακριβώς η κατάσταση μπορεί να οδηγήσει σε σφάλματα. Εάν ένα μέρος του κώδικά μας λειτουργεί με την παραδοχή ότι μια τιμή δεν θα αλλάξει ποτέ και ένα άλλο μέρος του κώδικά μας αλλάξει αυτήν την τιμή, είναι πιθανό το πρώτο μέρος του κώδικα να μην κάνει αυτό που σχεδιάστηκε να κάνει. Η αιτία αυτού του είδους σφάλματος μπορεί να είναι δύσκολο να εντοπιστεί εκ των υστέρων, ειδικά όταν το δεύτερο κομμάτι κώδικα αλλάζει την τιμή μόνο μερικές φορές. Ο μεταγλωττιστής Rust εγγυάται ότι όταν δηλώνετε ότι μια τιμή δεν θα αλλάξει, πραγματικά δεν θα αλλάξει, επομένως δεν χρειάζεται να την παρακολουθείτε μόνοι σας. Ο κώδικάς σας είναι επομένως ευκολότερος να συλλογιστεί.

It’s important that we get compile-time errors when we attempt to change a
value that’s designated as immutable because this very situation can lead to
bugs. If one part of our code operates on the assumption that a value will
never change and another part of our code changes that value, it’s possible
that the first part of the code won’t do what it was designed to do. The cause
of this kind of bug can be difficult to track down after the fact, especially
when the second piece of code changes the value only *sometimes*. The Rust
compiler guarantees that when you state that a value won’t change, it really
won’t change, so you don’t have to keep track of it yourself. Your code is thus
easier to reason through.

Αλλά η μεταβλητότητα μπορεί να είναι πολύ χρήσιμη και μπορεί να κάνει τον κώδικα πιο βολικό στη σύνταξη. Αν και οι μεταβλητές είναι αμετάβλητες από προεπιλογή, μπορείτε να τις κάνετε μεταβλητές προσθέτοντας mut μπροστά από το όνομα της μεταβλητής όπως κάνατε στο Κεφάλαιο 2. Η προσθήκη mut μεταφέρει επίσης την πρόθεση στους μελλοντικούς αναγνώστες του κώδικα, υποδεικνύοντας ότι άλλα μέρη του κώδικα θα αλλάξουν την τιμή αυτής της μεταβλητής.


But mutability can be very useful, and can make code more convenient to write.
Although variables are immutable by default, you can make them mutable by
adding `mut` in front of the variable name as you did in [Chapter
2][storing-values-with-variables]<!-- ignore -->. Adding `mut` also conveys
intent to future readers of the code by indicating that other parts of the code
will be changing this variable’s value.

Για παράδειγμα, ας αλλάξουμε το src/main.rs ως εξής:

For example, let’s change *src/main.rs* to the following:

<span class="filename">Όνομα αρχείου: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-02-adding-mut/src/main.rs}}
```

Όταν εκτελούμε το πρόγραμμα τώρα, παίρνουμε αυτό:
When we run the program now, we get this:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-02-adding-mut/output.txt}}
```
Επιτρέπεται να αλλάξουμε την τιμή δεσμευμένη σε x από 5 σε 6 όταν χρησιμοποιείται mut. Τελικά, το να αποφασίσετε εάν θα χρησιμοποιήσετε τη δυνατότητα μεταβλητότητας ή όχι εξαρτάται από εσάς και εξαρτάται από το τι πιστεύετε ότι είναι πιο ξεκάθαρο στη συγκεκριμένη περίπτωση.

We’re allowed to change the value bound to `x` from `5` to `6` when `mut` is
used. Ultimately, deciding whether to use mutability or not is up to you and
depends on what you think is clearest in that particular situation.

### Σταθερές

Όπως οι αμετάβλητες μεταβλητές, οι σταθερές είναι τιμές που συνδέονται με ένα όνομα και δεν επιτρέπεται να αλλάξουν, αλλά υπάρχουν μερικές διαφορές μεταξύ σταθερών και μεταβλητών.
Like immutable variables, *constants* are values that are bound to a name and
are not allowed to change, but there are a few differences between constants
and variables.

Πρώτον, δεν επιτρέπεται να χρησιμοποιείτε mut με σταθερές. Οι σταθερές δεν είναι απλώς αμετάβλητες από προεπιλογή - είναι πάντα αμετάβλητες. Δηλώνετε σταθερές χρησιμοποιώντας τη λέξη-κλειδί const αντί για τη λέξη-κλειδί let και ο τύπος της τιμής πρέπει να σχολιάζεται. Θα καλύψουμε τύπους και σχολιασμούς τύπων στην επόμενη ενότητα, "Τύποι δεδομένων", οπότε μην ανησυχείτε για τις λεπτομέρειες αυτήν τη στιγμή. Απλά να ξέρετε ότι πρέπει πάντα να σχολιάζετε τον τύπο.

First, you aren’t allowed to use `mut` with constants. Constants aren’t just
immutable by default—they’re always immutable. You declare constants using the
`const` keyword instead of the `let` keyword, and the type of the value *must*
be annotated. We’ll cover types and type annotations in the next section,
[“Data Types,”][data-types]<!-- ignore -->, so don’t worry about the details
right now. Just know that you must always annotate the type.

Οι σταθερές μπορούν να δηλωθούν σε οποιοδήποτε εύρος, συμπεριλαμβανομένου του καθολικού εύρους, γεγονός που τις καθιστά χρήσιμες για τιμές που πολλά μέρη του κώδικα πρέπει να γνωρίζουν.

Constants can be declared in any scope, including the global scope, which makes
them useful for values that many parts of code need to know about.

Η τελευταία διαφορά είναι ότι οι σταθερές μπορούν να οριστούν μόνο σε μια σταθερή έκφραση, όχι ως αποτέλεσμα μιας τιμής που θα μπορούσε να υπολογιστεί μόνο κατά το χρόνο εκτέλεσης.

The last difference is that constants may be set only to a constant expression,
not the result of a value that could only be computed at runtime.

Ακολουθεί ένα παράδειγμα σταθερής δήλωσης:
Here’s an example of a constant declaration:

```rust
const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
```


Το όνομα της σταθεράς είναι THREE_HOURS_IN_SECONDS και η τιμή της ορίζεται ως αποτέλεσμα πολλαπλασιασμού του 60 (ο αριθμός των δευτερολέπτων σε ένα λεπτό) με το 60 (ο αριθμός των λεπτών σε μια ώρα) με το 3 (ο αριθμός των ωρών που θέλουμε να μετρήσουμε σε αυτό το πρόγραμμα ). Η σύμβαση ονομασίας του Rust για τις σταθερές είναι η χρήση όλων των κεφαλαίων με υπογράμμιση μεταξύ των λέξεων. Ο μεταγλωττιστής είναι σε θέση να αξιολογήσει ένα περιορισμένο σύνολο λειτουργιών κατά το χρόνο μεταγλώττισης, γεγονός που μας επιτρέπει να επιλέξουμε να γράψουμε αυτήν την τιμή με τρόπο που είναι ευκολότερο να κατανοηθεί και να επαληθευτεί, αντί να ορίσουμε αυτήν τη σταθερά στην τιμή 10.800. Ανατρέξτε στην ενότητα της Αναφοράς σκουριάς σχετικά με τη συνεχή αξιολόγηση για περισσότερες πληροφορίες σχετικά με τις λειτουργίες που μπορούν να χρησιμοποιηθούν κατά τη δήλωση σταθερών.

The constant’s name is `THREE_HOURS_IN_SECONDS` and its value is set to the
result of multiplying 60 (the number of seconds in a minute) by 60 (the number
of minutes in an hour) by 3 (the number of hours we want to count in this
program). Rust’s naming convention for constants is to use all uppercase with
underscores between words. The compiler is able to evaluate a limited set of
operations at compile time, which lets us choose to write out this value in a
way that’s easier to understand and verify, rather than setting this constant
to the value 10,800. See the [Rust Reference’s section on constant
evaluation][const-eval] for more information on what operations can be used
when declaring constants.


Οι σταθερές ισχύουν για όλη τη διάρκεια εκτέλεσης ενός προγράμματος, εντός του πεδίου εφαρμογής στο οποίο δηλώθηκαν. Αυτή η ιδιότητα καθιστά χρήσιμες σταθερές για τιμές στον τομέα της εφαρμογής σας για τις οποίες μπορεί να χρειάζεται να γνωρίζουν πολλά μέρη του προγράμματος, όπως ο μέγιστος αριθμός πόντων που επιτρέπεται να κερδίσει ένας παίκτης ενός παιχνιδιού ή η ταχύτητα του φωτός.


Constants are valid for the entire time a program runs, within the scope in
which they were declared. This property makes constants useful for values in
your application domain that multiple parts of the program might need to know
about, such as the maximum number of points any player of a game is allowed to
earn, or the speed of light.

Η ονομασία των σκληρών κωδικοποιημένων τιμών που χρησιμοποιούνται σε όλο το πρόγραμμά σας ως σταθερές είναι χρήσιμη για τη μετάδοση της σημασίας αυτής της τιμής στους μελλοντικούς συντηρητές του κώδικα. Βοηθά επίσης να έχετε μόνο μία θέση στον κώδικά σας που θα πρέπει να αλλάξετε εάν η τιμή του σκληρού κωδικού έπρεπε να ενημερωθεί στο μέλλον.


Naming hardcoded values used throughout your program as constants is useful in
conveying the meaning of that value to future maintainers of the code. It also
helps to have only one place in your code you would need to change if the
hardcoded value needed to be updated in the future.

### Shadowing

Σκίαση

Όπως είδατε στο σεμινάριο του παιχνιδιού εικασίας στο Κεφάλαιο 2, μπορείτε να δηλώσετε μια νέα μεταβλητή με το ίδιο όνομα με μια προηγούμενη μεταβλητή. Οι Rustaceans λένε ότι η πρώτη μεταβλητή σκιάζεται από τη δεύτερη, πράγμα που σημαίνει ότι η δεύτερη μεταβλητή είναι αυτό που θα δει ο μεταγλωττιστής όταν χρησιμοποιείτε το όνομα της μεταβλητής. Στην πραγματικότητα, η δεύτερη μεταβλητή επισκιάζει την πρώτη, λαμβάνοντας οποιεσδήποτε χρήσεις του ονόματος της μεταβλητής για τον εαυτό της μέχρι είτε να σκιαστεί η ίδια είτε να τελειώσει το εύρος. Μπορούμε να σκιάζουμε μια μεταβλητή χρησιμοποιώντας το όνομα της ίδιας μεταβλητής και επαναλαμβάνοντας τη χρήση της λέξης-κλειδιού let ως εξής:



As you saw in the guessing game tutorial in [Chapter
2][comparing-the-guess-to-the-secret-number]<!-- ignore -->, you can declare a
new variable with the same name as a previous variable. Rustaceans say that the
first variable is *shadowed* by the second, which means that the second
variable is what the compiler will see when you use the name of the variable.
In effect, the second variable overshadows the first, taking any uses of the
variable name to itself until either it itself is shadowed or the scope ends.
We can shadow a variable by using the same variable’s name and repeating the
use of the `let` keyword as follows:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-03-shadowing/src/main.rs}}
```
Όνομα αρχείου: src/main.rs

{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-03-shadowing/src/main.rs}}
Αυτό το πρόγραμμα δεσμεύει πρώτα το x σε μια τιμή 5. Στη συνέχεια, δημιουργεί μια νέα μεταβλητή x επαναλαμβάνοντας έστω x =, λαμβάνοντας την αρχική τιμή και προσθέτοντας 1, ώστε η τιμή του x να είναι στη συνέχεια 6. Στη συνέχεια, μέσα σε ένα εσωτερικό πεδίο που δημιουργήθηκε με το curly αγκύλες, η τρίτη πρόταση let σκιάζει επίσης το x και δημιουργεί μια νέα μεταβλητή, πολλαπλασιάζοντας την προηγούμενη τιμή επί 2 για να δώσει x μια τιμή 12. Όταν τελειώσει αυτό το πεδίο, η εσωτερική σκίαση τελειώνει και το x επιστρέφει στο 6. Όταν εκτελούμε αυτό το πρόγραμμα , θα παράγει τα εξής:


This program first binds `x` to a value of `5`. Then it creates a new variable
`x` by repeating `let x =`, taking the original value and adding `1` so the
value of `x` is then `6`. Then, within an inner scope created with the curly
brackets, the third `let` statement also shadows `x` and creates a new
variable, multiplying the previous value by `2` to give `x` a value of `12`.
When that scope is over, the inner shadowing ends and `x` returns to being `6`.
When we run this program, it will output the following:
{{#include ../listings/ch03-common-programming-concepts/no-listing-03-shadowing/output.txt}}
Η σκίαση διαφέρει από την επισήμανση μιας μεταβλητής ως mut, επειδή θα λάβουμε ένα σφάλμα χρόνου μεταγλώττισης εάν προσπαθήσουμε κατά λάθος να αντιστοιχίσουμε ξανά σε αυτήν τη μεταβλητή χωρίς να χρησιμοποιήσουμε τη λέξη-κλειδί let. Χρησιμοποιώντας let, μπορούμε να εκτελέσουμε μερικούς μετασχηματισμούς σε μια τιμή, αλλά η μεταβλητή να είναι αμετάβλητη μετά την ολοκλήρωση αυτών των μετασχηματισμών.



```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-03-shadowing/output.txt}}
```

Shadowing is different from marking a variable as `mut` because we’ll get a
compile-time error if we accidentally try to reassign to this variable without
using the `let` keyword. By using `let`, we can perform a few transformations
on a value but have the variable be immutable after those transformations have
been completed.

Η άλλη διαφορά μεταξύ mut και shadowing είναι ότι επειδή δημιουργούμε ουσιαστικά μια νέα μεταβλητή όταν χρησιμοποιούμε ξανά τη λέξη-κλειδί let, μπορούμε να αλλάξουμε τον τύπο της τιμής αλλά να χρησιμοποιήσουμε ξανά το ίδιο όνομα. Για παράδειγμα, ας πούμε ότι το πρόγραμμά μας ζητά από έναν χρήστη να δείξει πόσα κενά θέλει μεταξύ κάποιου κειμένου εισάγοντας χαρακτήρες διαστήματος και, στη συνέχεια, θέλουμε να αποθηκεύσουμε αυτήν την είσοδο ως αριθμό:



The other difference between `mut` and shadowing is that because we’re
effectively creating a new variable when we use the `let` keyword again, we can
change the type of the value but reuse the same name. For example, say our
program asks a user to show how many spaces they want between some text by
inputting space characters, and then we want to store that input as a number:

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-04-shadowing-can-change-types/src/main.rs:here}}
```

Η πρώτη μεταβλητή διαστημάτων είναι τύπος συμβολοσειράς και η δεύτερη μεταβλητή διαστημάτων είναι τύπος αριθμού. Έτσι, η σκίαση μας γλιτώνει από το να βρούμε διαφορετικά ονόματα, όπως spaces_str και spaces_num. Αντίθετα, μπορούμε να χρησιμοποιήσουμε ξανά το όνομα απλούστερων διαστημάτων. Ωστόσο, αν προσπαθήσουμε να χρησιμοποιήσουμε το mut για αυτό, όπως φαίνεται εδώ, θα λάβουμε ένα σφάλμα χρόνου μεταγλώττισης:



The first `spaces` variable is a string type and the second `spaces` variable
is a number type. Shadowing thus spares us from having to come up with
different names, such as `spaces_str` and `spaces_num`; instead, we can reuse
the simpler `spaces` name. However, if we try to use `mut` for this, as shown
here, we’ll get a compile-time error:


```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-05-mut-cant-change-types/src/main.rs:here}}
```

Το σφάλμα λέει ότι δεν επιτρέπεται να αλλάξουμε τον τύπο μιας μεταβλητής:


The error says we’re not allowed to mutate a variable’s type:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-05-mut-cant-change-types/output.txt}}
```
Τώρα που εξερευνήσαμε πώς λειτουργούν οι μεταβλητές, ας δούμε περισσότερους τύπους δεδομένων που μπορούν να έχουν.
Now that we’ve explored how variables work, let’s look at more data types they
can have.

[comparing-the-guess-to-the-secret-number]:
ch02-00-guessing-game-tutorial.html#comparing-the-guess-to-the-secret-number
[data-types]: ch03-02-data-types.html#data-types
[storing-values-with-variables]: ch02-00-guessing-game-tutorial.html#storing-values-with-variables
[const-eval]: ../reference/const_eval.html

