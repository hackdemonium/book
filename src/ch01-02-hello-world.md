## Hello, World!

Αφού εγκαταστήσαμε την Rust, ήρθε η ώρα να γράψουμε το πρώτο μας πρόγραμμα.
Είναι παράδοση, όταν μαθαίνουμε μια καινούργια γλώσσα προγραμματισμού, να
γράφουμε ένα μικρό προγραμματάκι που εκτυπώνει στην οθόνη το κείμενο `Hello, world!`,
οπότε ας κάνουμε το ίδιο και εδώ.

> Σημείωση: Αυτό το βιβλίο προϋποθέτει βασική εξοικείωση με τη γραμμή εντολών. 
> H Rust δεν έχει συγκεκριμένες απαιτήσεις σχετικά με τρόπο επεξεργασίας ή τα εργαλεία σας 
> ή που βρίσκεται ο κώδικάς σας, επομένως εάν προτιμάτε να χρησιμοποιήσετε ένα ολοκληρωμένο 
> περιβάλλον ανάπτυξης (IDE) αντί για τη γραμμή εντολών, μη διστάσετε να χρησιμοποιήσετε το 
> αγαπημένο σας IDE. Πολλά IDE έχουν πλέον κάποιο βαθμό υποστήριξης Rust. ελέγξτε την τεκμηρίωση 
> του IDE για λεπτομέρειες. Η ομάδα της Rust έχει επικεντρωθεί στην παροχή εξαιρετικής υποστήριξης
>  IDE μέσω του `rust-analyzer`.
> [Appendix D][devtools]<!-- ignore --> για περισσότερες λεπτομέρειες..

### Δημιουργία ένος Φακέλου Προγραμμάτων

Θα ξεκινήσουμε πρώτα φτίαχνοντας έναν φάκελο για να αποθηκεύουμε τον κώδικα της Rust.
Είπαμε ότι για την Rust δεν έχει σημασία που βρίσκεται ο κώδικας, αλλά για τις ασκήσεις
και τα προγράμματα αυτού του βιβλίου, προτείνουμε να δημιουργήσετε έναν φάκελο *projects* 
στον φάκελο home directory και να κρατάτε όλα τα προγράμματά σας εκεί.

Ανοίξτε τον τερματικό και εισαγάγατε τις ακόλουθες εντολές για να δημιουργήσετε τον φάκελο *projects*
και έναν φάκελο για το πρόγραμμα “Hello, world!”, εντός του φακέλου *projects*.

Για το Linux, macOS, και το PowerShell των Windows, πληκτρολογήστε τα κάτωθι:

```console
$ mkdir ~/projects
$ cd ~/projects
$ mkdir hello_world
$ cd hello_world
```

Για το Windows CMD:

```cmd
> mkdir "%USERPROFILE%\projects"
> cd /d "%USERPROFILE%\projects"
> mkdir hello_world
> cd hello_world
```

### Συγγραφή και εκτέλεση ενός προγράμματος Rust

Στη συνέχεια δημιουργήστε ένα νέο αρχείο πηγιαίου κώδικα και ονομάστε το *main.rs*. 
Τα αρχεία της Rust πάντα τελειώνουν με την επέκταση *.rs*. Αν έχουμε πάνω από
μία λέξη στο όνομα του αρχείου, η σύμβαση είναι να χρησιμοποιούμε μία κάτω παύλα για να
τις ξεχωρίζουμε. Για παράδειγμα χρησιμοποιείστε *hello_world.rs* παρά *helloworld.rs*.

Τώρα ανοίξτε το αρχείο *main.rs* που μόλις δημιουργήσατε και πληκτρολογήστε τον κώδικά στο Listing 1-1.

<span class="filename">Filename: main.rs</span>

```rust
fn main() {
    println!("Hello, world!");
}
```

<span class="caption">Listing 1-1: Ένα πρόγραμμα που εκτυπώνει `Hello, world!`</span>

Σώστε το αρχείο και επιστρέψτε στο παράθυρο του τερματικού στον κατάλογο
*~/projects/hello_world*. Στο Linux η΄ macOS, enter the following
*~/projects/hello_world* directory. On Linux ή το macOS, πληκτρολογήστε τις ακόλουθες εντολές
για να μεταγλωττίσετε και να εκτελέσετε το αρχείο.

```console
$ rustc main.rs
$ ./main
Hello, world!
```

Στο Windows, δώστε την εντολή `.\main.exe` αντί για `./main`:

```powershell
> rustc main.rs
> .\main.exe
Hello, world!
```

Ανεξάρτητα από το λειτουργικό σας σύστημα, η θα πρέπει να εκτυπωθεί στον τερματικό
η συμβλοσειρά `Hello, world!`. Αν δεν δείτε να εμφανίζεται αυτό, ανατρέξτε στο τμήμα
[“Troubleshooting”][troubleshooting]<!-- ignore --> του κεφαλαίου Εγκατάσταση  για να
βρείτε βοήθεια.

Εάν εκτυπώθηκε το `Hello, world!`, συγχαρητήρια! Έχετε γράψει και επίσημα ένα πρόγραμμα
Rust. Αυτό σας κάνει προγραμματιστή Rust—Καλώς ορίσατε!

### Η Ανατομία ενός προγράμματος Rust 

Ας εξετάσουμε αναλυτικά το πρόγραμμα “Hello, world!”. Αυτό είναι το πρώτο κομμάτι του παζλ:

```rust
fn main() {

}
```
Οι παραπάνω γραμμές ορίζουν μία συνάρτηση με το όνομα `main`. Η συνάρτηση  `main` είναι ξεχωριστή,
είναι πάντα το πρώτο κομμμάτι κώδικα που τρέχει σε κάθε εκτελέσιμο πρόγραμμα της Rust. Εδώ οι πρώτες
γραμμές δηλώνουν μια συνάρτηση, όπως έιπαμε, με το όνομα `main`, η οποπια δεν έχει παραμέτρους, ούτε 
επιστρέφιε κάτι. Αν είχε παραμέτρους αυτοί θα ήταν μέσα στις παρενθέσεις `()`.

Το σώμα της συνάρτησης είναι περίκλειστο μέσα σε `{}`. Η Rust απαιτέι αγκύλες γύρω από όλα τα σώματα
συναρτήσεων. Είναι σωστή πρακτική να τοποθετούμε την αγκύλη στην ίδια γραμμή με το δηλωτικό της συνάρτησης,
αφήνοντας ένα κενό διάστημα μεταξύ τους.

> Note: If you want to stick to a standard style across Rust projects, you can
> use an automatic formatter tool called `rustfmt` to format your code in a
> particular style (more on `rustfmt` in
> [Appendix D][devtools]<!-- ignore -->). The Rust team has included this tool
> with the standard Rust distribution, as `rustc` is, so it should already be
> installed on your computer!

The body of the `main` function holds the following code:

```rust
    println!("Hello, world!");
```

This line does all the work in this little program: it prints text to the
screen. There are four important details to notice here.

First, Rust style is to indent with four spaces, not a tab.

Second, `println!` calls a Rust macro. If it had called a function instead, it
would be entered as `println` (without the `!`). We’ll discuss Rust macros in
more detail in Chapter 19. For now, you just need to know that using a `!`
means that you’re calling a macro instead of a normal function and that macros
don’t always follow the same rules as functions.

Third, you see the `"Hello, world!"` string. We pass this string as an argument
to `println!`, and the string is printed to the screen.

Fourth, we end the line with a semicolon (`;`), which indicates that this
expression is over and the next one is ready to begin. Most lines of Rust code
end with a semicolon.

### Compiling and Running Are Separate Steps

You’ve just run a newly created program, so let’s examine each step in the
process.

Before running a Rust program, you must compile it using the Rust compiler by
entering the `rustc` command and passing it the name of your source file, like
this:

```console
$ rustc main.rs
```

If you have a C or C++ background, you’ll notice that this is similar to `gcc`
or `clang`. After compiling successfully, Rust outputs a binary executable.

On Linux, macOS, and PowerShell on Windows, you can see the executable by
entering the `ls` command in your shell:

```console
$ ls
main  main.rs
```

On Linux and macOS, you’ll see two files. With PowerShell on Windows, you’ll
see the same three files that you would see using CMD. With CMD on Windows, you
would enter the following:

```cmd
> dir /B %= the /B option says to only show the file names =%
main.exe
main.pdb
main.rs
```

This shows the source code file with the *.rs* extension, the executable file
(*main.exe* on Windows, but *main* on all other platforms), and, when using
Windows, a file containing debugging information with the *.pdb* extension.
From here, you run the *main* or *main.exe* file, like this:

```console
$ ./main # or .\main.exe on Windows
```

If your *main.rs* is your “Hello, world!” program, this line prints `Hello,
world!` to your terminal.

If you’re more familiar with a dynamic language, such as Ruby, Python, or
JavaScript, you might not be used to compiling and running a program as
separate steps. Rust is an *ahead-of-time compiled* language, meaning you can
compile a program and give the executable to someone else, and they can run it
even without having Rust installed. If you give someone a *.rb*, *.py*, or
*.js* file, they need to have a Ruby, Python, or JavaScript implementation
installed (respectively). But in those languages, you only need one command to
compile and run your program. Everything is a trade-off in language design.

Just compiling with `rustc` is fine for simple programs, but as your project
grows, you’ll want to manage all the options and make it easy to share your
code. Next, we’ll introduce you to the Cargo tool, which will help you write
real-world Rust programs.

[troubleshooting]: ch01-01-installation.html#troubleshooting
[devtools]: appendix-04-useful-development-tools.md
