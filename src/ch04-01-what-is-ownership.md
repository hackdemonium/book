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

Η μεταβλητή `s`  σε μία καθορισμένη συμβολοσειρά, η τιμή της οποίας έχει κωδικοποιηθεί επακριβώς στο κείμενο του προγράμματός μας.
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

Θα επικεντρωθούμε στα μέρη της Συμβλοσειράς `String` που σχετίζονται με την ιδιοκτησία. Αυτές οι πτυχές ισχύουν επίσης για άλλους σύνθετους τύπους δεδομένων, είτε παρέχονται από την τυπική βιβλιοθήκη είτε δημιουργήθηκαν από εσάς. Θα συζητήσουμε τη `String` σε μεγαλύτερο βάθος στο [Chapter 8][ch8]<!-- ignore -->.

Έχουμε ήδη δει συμβολοσειρές, όπου η τιμή τους έχει καθοριστεί στο πρόγραμμά μας. Οι καθορισμένες συμβολοσειρές είναι βολικές, αλλά δεν είναι κατάλληλες για κάθε περίπτωση στην οποία μπορεί να θέλουμε να χρησιμοποιήσουμε κείμενο. Ένας λόγος είναι ότι είναι αμετάβλητες. Ένα άλλο είναι ότι δεν μπορεί να γίνει γνωστή κάθε τιμή συμβολοσειράς όταν γράφουμε τον κώδικά μας: για παράδειγμα, τι γίνεται αν θέλουμε να πάρουμε τα στοιχεία του χρήστη και να τα αποθηκεύσουμε; Για αυτές τις περιπτώσεις, η Rust έχει έναν δεύτερο τύπο συμβολοσειρών, το `String`. Αυτός ο τύπος διαχειρίζεται τα δεδομένα που εκχωρούνται στο σωρό και ως εκ τούτου είναι σε θέση να αποθηκεύσει μια ποσότητα κειμένου που είναι άγνωστη σε εμάς κατά τη στιγμή της μεταγλώττισης. Μπορείτε να δημιουργήσετε `String` από μια σειρά συμβόλων χρησιμοποιώντας τη συνάρτηση `from`, όπως εδώ:

```rust
let s = String::from("hello");
```
Ο τελεστής διπλής άνω και κάτω τελείας `::` μας επιτρέπει ονοματοθέσουμε αυτή τη συγκεκριμένη συνάρτηση κάτω από τον τύπο `String` αντί να χρησιμοποιήσουμε κάποιο είδος ονόματος όπως `string_from`. Θα συζητήσουμε αυτήν τη σύνταξη περισσότερο στην ενότητα [“Method Syntax”][method-syntax]<!-- ignore --> του Κεφαλαίου 5 και όταν μιλάμε για χώρους ονομάτων με δομοστοιχεία στο “Paths for Referring to an Item in the Module Tree”][paths-module-tree]<!-- ignore --> του Κεφαλαίου 7.

Αυτό το είδος συμβολοσειράς *μπορεί* να μεταβληθεί:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-01-can-mutate-string/src/main.rs:here}}
```
Λοιπόν, ποια είναι η διαφορά εδώ; Γιατί η `String` μπορεί να μεταβληθεί αλλά η καθορισμένη συμβολοσειρά όχι; Η διαφορά έγκειται στο πώς αυτοί οι δύο τύποι πραγματεύονται τη μνήμη.

### Memory and Allocation Μνήμη και Εκχώρηση

Στην περίπτωση μιας καθορισμένης συμβολοσειράς, γνωρίζουμε τα περιεχόμενα τη στιγμή της μεταγλώττισης, επομένως το κείμενο κωδικοποιείται απευθείας 
στο τελικό εκτελέσιμο αρχείο. Αυτός είναι ο λόγος που τα string literals είναι γρήγορα και αποτελεσματικά. Αλλά αυτές οι ιδιότητες προέρχονται μόνο 
από την αμετάβλητη καθορισμένη συμβολοσειρά. Δυστυχώς, δεν μπορούμε να βάλουμε ένα τμήμα μνήμης στο δυαδικό αρχείο για κάθε κομμάτι κειμένου του οποίου 
το μέγεθος είναι άγνωστο κατά τη στιγμή της μεταγλώττισης και του οποίου το μέγεθος μπορεί να αλλάξει κατά την εκτέλεση του προγράμματος.


Με τον τύπο `String`, για να υποστηρίξουμε ένα ευμετάβλητο, αναπτυσσόμενο κομμάτι κειμένου, πρέπει να εκχωρήσουμε μια ποσότητα μνήμης στο σωρό, άγνωστη 
τη στιγμή της μεταγλώττισης, για να κρατήσουμε τα περιεχόμενα. Αυτό σημαίνει:

* Η μνήμη πρέπει να ζητηθεί από τον εκχωρητή μνήμης κατά το χρόνο εκτέλεσης.
* Χρειαζόμαστε έναν τρόπο επιστροφής αυτής της μνήμης στον εκχωρητή όταν τελειώσουμε με το `String` μας.

Αυτό το πρώτο μέρος γίνεται από εμάς: όταν καλούμε το `String::from`, η υλοποίησή του ζητά τη μνήμη που χρειάζεται. 
Αυτό είναι σχεδόν καθολικό στις γλώσσες προγραμματισμού.

Ωστόσο, το δεύτερο μέρος διαφέρει. Σε γλώσσες με συλλέκτη απορριμμάτων *garbage collector (GC)*, το GC παρακολουθεί και καθαρίζει τη μνήμη που δεν
χρησιμοποιείται πλέον και δεν χρειάζεται να έχουμε το νου μας. Στις περισσότερες γλώσσες χωρίς GC, είναι δική μας ευθύνη να προσδιορίσουμε πότε η μνήμη δεν
χρησιμοποιείται πλέον και να καλέσουμε κώδικα για να την ελευθερώσουμε ρητά, όπως ακριβώς κάναμε όταν το ζητήσαμε. Το να γίνει αυτό σωστά ήταν ιστορικά ένα
δύσκολο πρόβλημα προγραμματισμού. Αν ξεχάσουμε, θα σπαταλήσουμε τη μνήμη. Αν το κάνουμε πολύ νωρίς, θα έχουμε μια μη έγκυρη μεταβλητή. Αν το κάνουμε δύο φορές, αυτό είναι επίσης ένα σφάλμα. Πρέπει να ζευγοποιήσουμε ακριβώς μία εκχώρηση `allocate` με ακριβώς μία απελευθέρωση `free`.

Η Rust ακολουθεί διαφορετική προσέγγιση: η μνήμη επιστρέφεται αυτόματα μόλις η μεταβλητή που την κατέχει βγει εκτός πεδίου δράσης. Ακολουθεί μια έκδοση του παραδείγματος του πεδίου εφαρμογής μας από το Listing 4-1 χρησιμοποιώντας ένα `String` αντί για μια καθορισμένη συμβολοσειρά:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-02-string-scope/src/main.rs:here}}
```
Υπάρχει ένα φυσικό σημείο στο οποίο μπορούμε να επιστρέψουμε τη μνήμη που χρειάζεται το `String` μας στον εκχωρητή: όταν το `s` ξεφεύγει από το πεδίο
δράσης του. Όταν μια μεταβλητή ξεφεύγει από το πεδίο δράσης της, η Rust καλεί μια ειδική συνάρτηση για εμάς. Αυτή η συνάρτηση ονομάζεται 
[`drop`][drop]<!-- ignore -->, και είναι όπου ο δημιουργός του `String` μπορεί να βάλει τον κώδικα για να επιστρέψει τη μνήμη. Η Rust καλεί αυτόματα την `drop` στην αγκύλη κλεισίματος.

> Σημείωση: Στη C++, αυτό το μοτίβο ανακατανομής πόρων στο τέλος της διάρκειας ζωής ενός στοιχείου ονομάζεται
> *Resource Acquisition Is Initialization (RAII)*.
> Η συνάρτηση `drop` στη Rust θα σας είναι οικεία εάν έχετε χρησιμοποιήσει μοτίβα RAII.

Αυτό το μοτίβο έχει βαθιά επίδραση στον τρόπο με τον οποίο γράφεται ο κώδικας Rust. Μπορεί να φαίνεται απλό αυτή τη στιγμή, αλλά η συμπεριφορά του 
κώδικα μπορεί να είναι απροσδόκητη σε πιο περίπλοκες καταστάσεις όταν θέλουμε πολλαπλές μεταβλητές να χρησιμοποιήσουν τα δεδομένα που έχουμε εκχωρήσει 
στο σωρό. Ας εξερευνήσουμε τώρα μερικές από αυτές τις καταστάσεις.

<!-- Old heading. Do not remove or links may break. -->
<a id="ways-variables-and-data-interact-move"></a>

#### Μεταβλητές και Αλληλεπίδραση Δεδομένων με το Move

Πολλαπλές μεταβλητές μπορούν να αλληλεπιδράσουν με τα ίδια δεδομένα με διαφορετικούς τρόπους στη Rust. Ας δούμε ένα παράδειγμα χρησιμοποιώντας έναν ακέραιο 
στο Listing 4-2.

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-02/src/main.rs:here}}
```

<span class="caption">Listing 4-2: Εκχωρώντας την ακέραια τιμή της μεταβλητής `x` στην `y`</span>

Μπορούμε πιθανώς να μαντέψουμε τι κάνει αυτό: «δεσμεύστε την τιμή `5` στο `x`. στη συνέχεια δημιουργήστε ένα αντίγραφο της τιμής στο `x` και δεσμεύσετε 
την στην `y`." Τώρα έχουμε δύο μεταβλητές, `x` και `y`, και οι δύο είναι ίσες με `5`. Αυτό είναι επακριβώς αυτό που συμβαίνει, καθώς οι ακέραιοι 
αριθμοί είναι απλές τιμές με γνωστό, σταθερό μέγεθος, και αυτές οι δύο τιμές `5` ωθούνται στη στοίβα.

Τώρα ας δούμε την έκδοση με το `String`:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-03-string-move/src/main.rs:here}}
```
Δείχνει παρόμοιο, όποτε μπορεί να θεωρήσουμε ότι δουλεύει με τον ίδιο τρόπο: αυτό είναι, η δεύτερη γραμμή θα φτιάξει ένα σντίγραφο της τιμής της `s1` και
θα την δεσμεύσει στην `s2`. Όμως δεν είναι ακριβώς αυτό που σημβαίνει.

Ρίξτε μια ματιά στο Figure 4-1, για να δείτε τι συμβαίνει στον `String`, στο παρασκήνιο. Ένας `String` είναι δομημένος από τρια στοιχεία, τα οποία εμφαίνονται στα αριστερά: ένας δείκτης στη μνήμη που κρατάει τα περιεχόμενα της συμβολοσειράς, ένα μήκος και μία χωρητικότητα. Αυτή η ομάδα δεδομένων,
αποθηκεύεται στη στοίβα. Στα δεξιά είναι η μνήμη στον σωρό που κρατάει τα περιεχόμενα.


<img alt="Two tables: the first table contains the representation of s1 on the
stack, consisting of its length (5), capacity (5), and a pointer to the first
value in the second table. The second table contains the representation of the
string data on the heap, byte by byte." src="img/trpl04-01.svg" class="center"
style="width: 50%;" />

<span class="caption">Figure 4-1: Αναπαράσταση στη μνήμη ενός `String` που κρατάει την δεσμευμένη στην `s1` τιμή `"hello"`</span>

Tο μήκος είναι το πόσο μνήμη, σε bytes, χρησιμοποιούν τα περιεχόμενα του `String`. Η χωρητικότητα  είναι η συνολική ποσότητα μνήμης που έλαβε το `String`
από τον εκχωρητή. Η διαφορά μεταξύ του μήκους και της χωρητικότητας έχει σημασία, αλλά όχι στο πλαίσιο αυτό, οπότε για την ώρα, είναι εντάξει αν αγνήσουμε
τη χωρητικότητα.

Όταν εκχωρούμε το `s1` στο `s2` τα δεδομένα του `String` αντιγράφονται, εννοώντας ότι αντιγράφουμε τον δείκτη, το μήκος και την χωρητικότητα, που βρίσκονται
στη στοίβα. Δεν αντιγράφουμε τα δεδομένα του σωρου, στα οποία αναφέρεται ο δείκτης. Με άλλα λόγια, η αναπαράσταση των δεδομένων στη μνήμη είναι όπως στο
Figure 4-2


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
