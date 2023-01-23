
## Mεταβλητές και Μεταβλητότητα

Όπως αναφέρθηκε στην ενότητα [“Storing Values with Variables”][storing-values-with-variables]<!-- ignore -->, από προεπιλογή, οι μεταβλητές είναι αμετάβλητες. Είναι μια από τις πολλές ωθήσεις που σας δίνει η Rust, ώστε να γράψετε τον κώδικά σας με τρόπο που να εκμεταλλεύεται την ασφάλεια και τον εύκολο συγχρονισμό που προσφέρει η Rust. Ωστόσο, εξακολουθείτε να έχετε την επιλογή να κάνετε τις μεταβλητές σας ευμετάβλητες. Ας εξερευνήσουμε πώς και γιατί η Rust σας ενθαρρύνει να προτιμάτε την αμετάβλητότητα και γιατί μερικές φορές μπορεί να θέλετε να κάνετε εξαίρεση.

Όταν μια μεταβλητή είναι αμετάβλητη, αν μια τιμή δεσμευτεί σε ένα όνομα, δεν μπορείτε να αλλάξετε αυτήν την τιμή. Για να το δείτε αυτό, δημιουργήστε ένα νέο προτζεκτ που ονομάζεται μεταβλητές στον κατάλογο των έργων σας χρησιμοποιώντας νέες μεταβλητές φορτίου.

When a variable is immutable, once a value is bound to a name, you can’t change
that value. To illustrate this, generate a new project called *variables* in
your *projects* directory by using `cargo new variables`.

Στη συνέχεια, στον νέο σας κατάλογο μεταβλητών, ανοίξτε το src/main.rs και αντικαταστήστε τον κώδικά του με τον ακόλουθο κώδικα, ο οποίος δεν θα μεταγλωττιστεί ακόμα:

Then, in your new *variables* directory, open *src/main.rs* and replace its
code with the following code, which won’t compile just yet:

<span class="filename">Όνομα αρχείου: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-01-variables-are-immutable/src/main.rs}}
```

Save and run the program using `cargo run`. You should receive an error message
regarding an immutability error, as shown in this output:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-01-variables-are-immutable/output.txt}}
```

This example shows how the compiler helps you find errors in your programs.
Compiler errors can be frustrating, but really they only mean your program
isn’t safely doing what you want it to do yet; they do *not* mean that you’re
not a good programmer! Experienced Rustaceans still get compiler errors.

You received the error message `` cannot assign twice to immutable variable `x`
`` because you tried to assign a second value to the immutable `x` variable.

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

But mutability can be very useful, and can make code more convenient to write.
Although variables are immutable by default, you can make them mutable by
adding `mut` in front of the variable name as you did in [Chapter
2][storing-values-with-variables]<!-- ignore -->. Adding `mut` also conveys
intent to future readers of the code by indicating that other parts of the code
will be changing this variable’s value.

For example, let’s change *src/main.rs* to the following:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-02-adding-mut/src/main.rs}}
```

When we run the program now, we get this:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-02-adding-mut/output.txt}}
```

We’re allowed to change the value bound to `x` from `5` to `6` when `mut` is
used. Ultimately, deciding whether to use mutability or not is up to you and
depends on what you think is clearest in that particular situation.

### Constants

Like immutable variables, *constants* are values that are bound to a name and
are not allowed to change, but there are a few differences between constants
and variables.

First, you aren’t allowed to use `mut` with constants. Constants aren’t just
immutable by default—they’re always immutable. You declare constants using the
`const` keyword instead of the `let` keyword, and the type of the value *must*
be annotated. We’ll cover types and type annotations in the next section,
[“Data Types,”][data-types]<!-- ignore -->, so don’t worry about the details
right now. Just know that you must always annotate the type.

Constants can be declared in any scope, including the global scope, which makes
them useful for values that many parts of code need to know about.

The last difference is that constants may be set only to a constant expression,
not the result of a value that could only be computed at runtime.

Here’s an example of a constant declaration:

```rust
const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
```

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

Constants are valid for the entire time a program runs, within the scope in
which they were declared. This property makes constants useful for values in
your application domain that multiple parts of the program might need to know
about, such as the maximum number of points any player of a game is allowed to
earn, or the speed of light.

Naming hardcoded values used throughout your program as constants is useful in
conveying the meaning of that value to future maintainers of the code. It also
helps to have only one place in your code you would need to change if the
hardcoded value needed to be updated in the future.

### Shadowing

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

This program first binds `x` to a value of `5`. Then it creates a new variable
`x` by repeating `let x =`, taking the original value and adding `1` so the
value of `x` is then `6`. Then, within an inner scope created with the curly
brackets, the third `let` statement also shadows `x` and creates a new
variable, multiplying the previous value by `2` to give `x` a value of `12`.
When that scope is over, the inner shadowing ends and `x` returns to being `6`.
When we run this program, it will output the following:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-03-shadowing/output.txt}}
```

Shadowing is different from marking a variable as `mut` because we’ll get a
compile-time error if we accidentally try to reassign to this variable without
using the `let` keyword. By using `let`, we can perform a few transformations
on a value but have the variable be immutable after those transformations have
been completed.

The other difference between `mut` and shadowing is that because we’re
effectively creating a new variable when we use the `let` keyword again, we can
change the type of the value but reuse the same name. For example, say our
program asks a user to show how many spaces they want between some text by
inputting space characters, and then we want to store that input as a number:

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-04-shadowing-can-change-types/src/main.rs:here}}
```

The first `spaces` variable is a string type and the second `spaces` variable
is a number type. Shadowing thus spares us from having to come up with
different names, such as `spaces_str` and `spaces_num`; instead, we can reuse
the simpler `spaces` name. However, if we try to use `mut` for this, as shown
here, we’ll get a compile-time error:

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-05-mut-cant-change-types/src/main.rs:here}}
```

The error says we’re not allowed to mutate a variable’s type:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-05-mut-cant-change-types/output.txt}}
```

Now that we’ve explored how variables work, let’s look at more data types they
can have.

[comparing-the-guess-to-the-secret-number]:
ch02-00-guessing-game-tutorial.html#comparing-the-guess-to-the-secret-number
[data-types]: ch03-02-data-types.html#data-types
[storing-values-with-variables]: ch02-00-guessing-game-tutorial.html#storing-values-with-variables
[const-eval]: ../reference/const_eval.html


Μεταβλητές και Μεταβλητότητα

Όπως αναφέρθηκε στην ενότητα "Αποθήκευση τιμών με μεταβλητές", από προεπιλογή, οι μεταβλητές είναι αμετάβλητες. Αυτό είναι ένα από τα πολλά ωθήσεις που σας δίνει η Rust για να γράψετε τον κώδικά σας με τρόπο που να εκμεταλλεύεται την ασφάλεια και τον εύκολο ταυτόχρονο που προσφέρει η Rust. Ωστόσο, εξακολουθείτε να έχετε την επιλογή να κάνετε τις μεταβλητές σας μεταβλητές. Ας εξερευνήσουμε πώς και γιατί το Rust σας ενθαρρύνει να προτιμάτε την αμετάβλητη και γιατί μερικές φορές μπορεί να θέλετε να εξαιρεθείτε.

Όταν μια μεταβλητή είναι αμετάβλητη, όταν μια τιμή δεσμευτεί σε ένα όνομα, δεν μπορείτε να αλλάξετε αυτήν την τιμή. Για να το δείξετε αυτό, δημιουργήστε ένα νέο έργο που ονομάζεται μεταβλητές στον κατάλογο των έργων σας χρησιμοποιώντας νέες μεταβλητές φορτίου.

Στη συνέχεια, στον νέο σας κατάλογο μεταβλητών, ανοίξτε το src/main.rs και αντικαταστήστε τον κώδικά του με τον ακόλουθο κώδικα, ο οποίος δεν θα μεταγλωττιστεί ακόμα:

Όνομα αρχείου: src/main.rs

{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-01-variables-are-immutable/src/main.rs}}
Αποθηκεύστε και εκτελέστε το πρόγραμμα χρησιμοποιώντας τη λειτουργία cargo run. Θα πρέπει να λάβετε ένα μήνυμα σφάλματος σχετικά με ένα σφάλμα μη μεταβλητότητας, όπως φαίνεται σε αυτό το αποτέλεσμα:

{{#include ../listings/ch03-common-programming-concepts/no-listing-01-variables-are-immutable/output.txt}}
Αυτό το παράδειγμα δείχνει πώς ο μεταγλωττιστής σάς βοηθά να βρείτε σφάλματα στα προγράμματά σας. Τα σφάλματα μεταγλωττιστή μπορεί να είναι απογοητευτικά, αλλά στην πραγματικότητα σημαίνουν μόνο ότι το πρόγραμμά σας δεν κάνει ακόμα με ασφάλεια αυτό που θέλετε να κάνει. δεν σημαίνουν ότι δεν είσαι καλός προγραμματιστής! Οι έμπειροι Rustaceans εξακολουθούν να λαμβάνουν σφάλματα μεταγλωττιστή.

Λάβατε το μήνυμα σφάλματος δεν είναι δυνατή η εκχώρηση δύο φορές στη αμετάβλητη μεταβλητή `x` επειδή προσπαθήσατε να εκχωρήσετε μια δεύτερη τιμή στη μεταβλητή αμετάβλητη x.

Είναι σημαντικό να λαμβάνουμε σφάλματα χρόνου μεταγλώττισης όταν προσπαθούμε να αλλάξουμε μια τιμή που έχει οριστεί ως αμετάβλητη, επειδή αυτή ακριβώς η κατάσταση μπορεί να οδηγήσει σε σφάλματα. Εάν ένα μέρος του κώδικά μας λειτουργεί με την παραδοχή ότι μια τιμή δεν θα αλλάξει ποτέ και ένα άλλο μέρος του κώδικά μας αλλάξει αυτήν την τιμή, είναι πιθανό το πρώτο μέρος του κώδικα να μην κάνει αυτό που σχεδιάστηκε να κάνει. Η αιτία αυτού του είδους σφάλματος μπορεί να είναι δύσκολο να εντοπιστεί εκ των υστέρων, ειδικά όταν το δεύτερο κομμάτι κώδικα αλλάζει την τιμή μόνο μερικές φορές. Ο μεταγλωττιστής Rust εγγυάται ότι όταν δηλώνετε ότι μια τιμή δεν θα αλλάξει, πραγματικά δεν θα αλλάξει, επομένως δεν χρειάζεται να την παρακολουθείτε μόνοι σας. Ο κώδικάς σας είναι επομένως ευκολότερος να συλλογιστεί.

Αλλά η μεταβλητότητα μπορεί να είναι πολύ χρήσιμη και μπορεί να κάνει τον κώδικα πιο βολικό στη σύνταξη. Αν και οι μεταβλητές είναι αμετάβλητες από προεπιλογή, μπορείτε να τις κάνετε μεταβλητές προσθέτοντας mut μπροστά από το όνομα της μεταβλητής όπως κάνατε στο Κεφάλαιο 2. Η προσθήκη mut μεταφέρει επίσης την πρόθεση στους μελλοντικούς αναγνώστες του κώδικα, υποδεικνύοντας ότι άλλα μέρη του κώδικα θα αλλάξουν την τιμή αυτής της μεταβλητής.

Για παράδειγμα, ας αλλάξουμε το src/main.rs ως εξής:

Όνομα αρχείου: src/main.rs

{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-02-adding-mut/src/main.rs}}
Όταν εκτελούμε το πρόγραμμα τώρα, παίρνουμε αυτό:

{{#include ../listings/ch03-common-programming-concepts/no-listing-02-adding-mut/output.txt}}
Επιτρέπεται να αλλάξουμε την τιμή δεσμευμένη σε x από 5 σε 6 όταν χρησιμοποιείται mut. Τελικά, το να αποφασίσετε εάν θα χρησιμοποιήσετε τη δυνατότητα μεταβλητότητας ή όχι εξαρτάται από εσάς και εξαρτάται από το τι πιστεύετε ότι είναι πιο ξεκάθαρο στη συγκεκριμένη περίπτωση.

Σταθερές

Όπως οι αμετάβλητες μεταβλητές, οι σταθερές είναι τιμές που συνδέονται με ένα όνομα και δεν επιτρέπεται να αλλάξουν, αλλά υπάρχουν μερικές διαφορές μεταξύ σταθερών και μεταβλητών.

Πρώτον, δεν επιτρέπεται να χρησιμοποιείτε mut με σταθερές. Οι σταθερές δεν είναι απλώς αμετάβλητες από προεπιλογή - είναι πάντα αμετάβλητες. Δηλώνετε σταθερές χρησιμοποιώντας τη λέξη-κλειδί const αντί για τη λέξη-κλειδί let και ο τύπος της τιμής πρέπει να σχολιάζεται. Θα καλύψουμε τύπους και σχολιασμούς τύπων στην επόμενη ενότητα, "Τύποι δεδομένων", οπότε μην ανησυχείτε για τις λεπτομέρειες αυτήν τη στιγμή. Απλά να ξέρετε ότι πρέπει πάντα να σχολιάζετε τον τύπο.

Οι σταθερές μπορούν να δηλωθούν σε οποιοδήποτε εύρος, συμπεριλαμβανομένου του καθολικού εύρους, γεγονός που τις καθιστά χρήσιμες για τιμές που πολλά μέρη του κώδικα πρέπει να γνωρίζουν.

Η τελευταία διαφορά είναι ότι οι σταθερές μπορούν να οριστούν μόνο σε μια σταθερή έκφραση, όχι ως αποτέλεσμα μιας τιμής που θα μπορούσε να υπολογιστεί μόνο κατά το χρόνο εκτέλεσης.

Ακολουθεί ένα παράδειγμα σταθερής δήλωσης:

const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
Το όνομα της σταθεράς είναι THREE_HOURS_IN_SECONDS και η τιμή της ορίζεται ως αποτέλεσμα πολλαπλασιασμού του 60 (ο αριθμός των δευτερολέπτων σε ένα λεπτό) με το 60 (ο αριθμός των λεπτών σε μια ώρα) με το 3 (ο αριθμός των ωρών που θέλουμε να μετρήσουμε σε αυτό το πρόγραμμα ). Η σύμβαση ονομασίας του Rust για τις σταθερές είναι η χρήση όλων των κεφαλαίων με υπογράμμιση μεταξύ των λέξεων. Ο μεταγλωττιστής είναι σε θέση να αξιολογήσει ένα περιορισμένο σύνολο λειτουργιών κατά το χρόνο μεταγλώττισης, γεγονός που μας επιτρέπει να επιλέξουμε να γράψουμε αυτήν την τιμή με τρόπο που είναι ευκολότερο να κατανοηθεί και να επαληθευτεί, αντί να ορίσουμε αυτήν τη σταθερά στην τιμή 10.800. Ανατρέξτε στην ενότητα της Αναφοράς σκουριάς σχετικά με τη συνεχή αξιολόγηση για περισσότερες πληροφορίες σχετικά με τις λειτουργίες που μπορούν να χρησιμοποιηθούν κατά τη δήλωση σταθερών.
