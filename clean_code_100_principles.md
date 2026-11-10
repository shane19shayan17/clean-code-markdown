# 100 Principles of Clean Code

Based on *Clean Code: A Handbook of Agile Software Craftsmanship* by Robert C. Martin (Uncle Bob).
Each principle lists the source chapter, a short rule, and a **bad** and a **good** C++ example.
Examples are short illustrations, not complete programs.

---

## Table of Contents

1. **Meaningful Names** (Principles 1–17)
2. **Functions** (Principles 18–39)
3. **Comments** (Principles 40–49)
4. **Formatting** (Principles 50–60)
5. **Objects and Data Structures** (Principles 61–65)
6. **Error Handling** (Principles 66–72)
7. **Boundaries** (Principles 73–75)
8. **Unit Tests** (Principles 76–81)
9. **Classes** (Principles 82–88)
10. **Systems** (Principles 89–92)
11. **Emergence** (Principles 93–96)
12. **Concurrency** (Principles 97–100)

---

# Part 1: Meaningful Names (Chapter 2)

## 1. Use Intention-Revealing Names
**Rule:** A name should tell why it exists, what it does, and how it is used. If a name needs a comment, rename it.

**Bad:**
```cpp
int d; // elapsed time in days
std::vector<std::vector<int>> getThem(const std::vector<std::vector<int>>& theList) {
    std::vector<std::vector<int>> list1;
    for (const auto& x : theList)
        if (x[0] == 4) list1.push_back(x);
    return list1;
}
```

**Good:**
```cpp
int elapsedTimeInDays;
std::vector<Cell> getFlaggedCells(const std::vector<Cell>& gameBoard) {
    std::vector<Cell> flaggedCells;
    for (const Cell& cell : gameBoard)
        if (cell.isFlagged()) flaggedCells.push_back(cell);
    return flaggedCells;
}
```

## 2. Avoid Disinformation
**Rule:** Do not use names that suggest a meaning the code does not have (for example, calling a non-list container a "list").

**Bad:**
```cpp
std::set<Account> accountList;   // it is a set, not a list
int O = 0;                       // O looks like 0
int l = 1;                       // l looks like 1
```

**Good:**
```cpp
std::set<Account> accountGroup;
int ownerCount = 0;
int lineCount = 1;
```

## 3. Make Meaningful Distinctions
**Rule:** If two names must differ, they must mean different things. Avoid number series and noise words.

**Bad:**
```cpp
void copyChars(char a1[], char a2[]);
class Product {};
class ProductInfo {};
class ProductData {};
```

**Good:**
```cpp
void copyChars(char* destination, const char* source);
class Product {};
class ProductPrice {};   // the name now says what is different
```

## 4. Use Pronounceable Names
**Rule:** Names should be words you can say in a code review. Avoid abbreviations that cannot be spoken.

**Bad:**
```cpp
struct DtaRcrd102 {
    Date genymdhms;
    Date modymdhms;
    std::string pszqint = "102";
};
```

**Good:**
```cpp
struct CustomerRecord {
    Date generationTimestamp;
    Date modificationTimestamp;
    std::string recordId = "102";
};
```

## 5. Use Searchable Names
**Rule:** Use named constants instead of magic numbers, and longer names for variables used across many places. Single letters are fine only in tiny scopes.

**Bad:**
```cpp
for (int j = 0; j < 34; j++) {
    s += (t[j] * 4) / 5;
}
```

**Good:**
```cpp
constexpr int NUMBER_OF_TASKS = 34;
constexpr int WORK_DAYS_PER_WEEK = 5;
constexpr int REAL_DAYS_PER_IDEAL_DAY = 4;

int sum = 0;
for (int j = 0; j < NUMBER_OF_TASKS; j++) {
    int realTaskDays = taskEstimate[j] * REAL_DAYS_PER_IDEAL_DAY;
    sum += realTaskDays / WORK_DAYS_PER_WEEK;
}
```

## 6. Avoid Encodings
**Rule:** Do not encode type or scope into names (Hungarian notation, `m_` prefixes). Modern compilers and IDEs already show this information.

**Bad:**
```cpp
class Part {
    std::string m_dsc;      // member prefix
    int iCount;             // type prefix
    char* pszName;          // Hungarian notation
};
```

**Good:**
```cpp
class Part {
    std::string description;
    int count;
    std::string name;
};
```

## 7. Avoid Mental Mapping
**Rule:** Readers should not have to translate your names into other names in their heads.

**Bad:**
```cpp
for (auto a : accounts) {
    auto b = a.c();        // what is b? what is c?
    total += b;
}
```

**Good:**
```cpp
for (const Account& account : accounts) {
    auto balance = account.currentBalance();
    total += balance;
}
```

## 8. Class Names Should Be Nouns
**Rule:** Classes are nouns or noun phrases. Avoid vague words like `Manager`, `Processor`, `Data`, `Info`.

**Bad:**
```cpp
class CustomerDataManager {};
class PaymentProcessor {};
```

**Good:**
```cpp
class CustomerStore {};
class PaymentHistory {};
```

## 9. Method Names Should Be Verbs
**Rule:** Methods are verbs. Accessors, mutators and predicates use `get`, `set`, `is` prefixes.

**Bad:**
```cpp
std::string name();
void name(const std::string& n);
bool posted();
void payment(Money m);
```

**Good:**
```cpp
std::string getName() const;
void setName(const std::string& n);
bool isPosted() const;
void postPayment(Money m);
```

## 10. Don't Be Cute
**Rule:** Choose clarity over humor or slang. Jokes are forgotten; the code stays.

**Bad:**
```cpp
void HolyHandGrenade();
void eatMyShorts();   // means abort()
```

**Good:**
```cpp
void deleteItems();
void abort();
```

## 11. Pick One Word per Concept
**Rule:** Use the same word for the same abstract idea across the whole code base.

**Bad:**
```cpp
user.fetch();
repository.retrieve();
cache.get();
```

**Good:**
```cpp
user.get();
repository.get();
cache.get();
```

## 12. Don't Pun
**Rule:** Do not use one word for two different meanings. If `add` concatenates values, do not use `add` to insert into a collection.

**Bad:**
```cpp
Money total = m1.add(m2);          // arithmetic: returns a new value
collection.add(item);              // insertion: different semantics
```

**Good:**
```cpp
Money total = m1.add(m2);
collection.insert(item);
```

## 13. Use Solution Domain Names
**Rule:** Programmers read the code. Use computer science terms (`Visitor`, `Queue`, `Factory`) when they describe the solution.

**Bad:**
```cpp
class ThingThatWalksAccounts {};
class ThingThatWaitsInLine {};
```

**Good:**
```cpp
class AccountVisitor {};   // VISITOR pattern
class JobQueue {};         // standard data structure name
```

## 14. Use Problem Domain Names
**Rule:** When no programming term fits, use the vocabulary of the business domain, so domain experts understand the code.

**Bad:**
```cpp
class DataBlob {
    double x;   // what is x?
};
```

**Good:**
```cpp
class InsuranceClaim {
    double claimAmount;
    double deductible;
};
```

## 15. Add Meaningful Context
**Rule:** Put variables into a well-named class or function so the context is clear. Do not rely on prefixes like `addrState`.

**Bad:**
```cpp
std::string addrFirstName, addrLastName, addrStreet, addrState, addrZip;
```

**Good:**
```cpp
struct Address {
    std::string firstName;
    std::string lastName;
    std::string street;
    std::string state;
    std::string zip;
};
```

## 16. Don't Add Gratuitous Context
**Rule:** Do not add prefixes that repeat the project name or add no information. Shorter names are better if they are still clear.

**Bad:**
```cpp
class GSDAccountAddress {};   // GSD is the app name; it adds nothing
```

**Good:**
```cpp
class MailingAddress {};
```

## 17. Use Long Names for Long Scopes, Short Names for Short Scopes
**Rule:** The length of a name should match how far it is visible. Loop counters can be short; global names should be descriptive.

**Bad:**
```cpp
int n;   // global, used in 300 lines, meaning unclear

void f() {
    for (int totalNumberOfCustomersInTheSystem = 0; ...) {}
}
```

**Good:**
```cpp
int totalNumberOfCustomers;   // global, descriptive

void printReport() {
    for (int i = 0; i < 10; ++i) {}   // local, short scope: fine
}
```

---

# Part 2: Functions (Chapter 3)

## 18. Functions Should Be Small
**Rule:** Functions should be very short, ideally a few lines. Shorter functions are easier to name, read and test.

**Bad:**
```cpp
void processOrder(const Order& order) {
    // 80 lines: validate, compute tax, update inventory,
    // write to database, send email, log everything...
}
```

**Good:**
```cpp
void processOrder(const Order& order) {
    validate(order);
    applyTax(order);
    updateInventory(order);
    saveOrder(order);
    notifyCustomer(order);
}
```

## 19. Blocks Should Be One Line (Call a Function)
**Rule:** The body of an `if`, `while`, or `for` should be a single function call. The call's name documents the intent.

**Bad:**
```cpp
if (isValid(order)) {
    total += order.amount();
    order.setStatus(Status::Paid);
    log("paid");
}
```

**Good:**
```cpp
if (isValid(order))
    markAsPaid(order);
```

## 20. Keep the Indent Level Low
**Rule:** Do not nest more than one or two levels. Use early returns or extracted functions.

**Bad:**
```cpp
void check(const User& u) {
    if (u.isActive()) {
        if (u.hasRole("admin")) {
            if (u.passwordExpired()) {
                notifyExpired(u);
            }
        }
    }
}
```

**Good:**
```cpp
void check(const User& u) {
    if (!u.isActive()) return;
    if (!u.hasRole("admin")) return;
    if (u.passwordExpired()) notifyExpired(u);
}
```

## 21. Functions Should Do One Thing
**Rule:** A function does one thing, and does it only. If you can extract another function whose name is not a restatement of the body, it does more than one thing.

**Bad:**
```cpp
void renderPageWithSetups(Page& page) {
    if (page.isTest()) {                 // thing 1: decide
        includeSetups(page);             // thing 2: include
    }
    page.render();                       // thing 3: render
}
```

**Good:**
```cpp
void renderPage(Page& page) {
    includeSetupsIfTestPage(page);
    page.render();
}
```

## 22. One Level of Abstraction per Function
**Rule:** All statements in a function should be at the same level of detail.

**Bad:**
```cpp
std::string buildReport(const Data& d) {
    std::string html = renderHeader(d);   // high level
    html += "<tr><td>";                   // low level HTML detail
    html += d.total();
    return html;
}
```

**Good:**
```cpp
std::string buildReport(const Data& d) {
    std::string html = renderHeader(d);
    html += renderTotalRow(d);
    return html;
}
```

## 23. Read Code Top to Bottom: The Stepdown Rule
**Rule:** Put each function above the functions it calls, so reading the file works like a set of descending "TO" paragraphs.

**Bad:**
```cpp
void saveCustomer();      // defined far below
void validateCustomer();
void run() {
    validateCustomer();
    saveCustomer();
}
```

**Good:**
```cpp
void run() {              // top: the story
    validateCustomer();   // next level down
    saveCustomer();
}
void validateCustomer();  // details below
void saveCustomer();
```

## 24. Bury Switch Statements Behind Polymorphism
**Rule:** A switch on type repeats for every new operation. Hide it in a factory and use virtual functions.

**Bad:**
```cpp
double calculatePay(const Employee& e) {
    switch (e.type) {
        case Type::Commissioned: return commissionedPay(e);
        case Type::Hourly:       return hourlyPay(e);
        case Type::Salaried:     return salariedPay(e);
    }
    throw std::invalid_argument("unknown type");
}
```

**Good:**
```cpp
class Employee {
public:
    virtual ~Employee() = default;
    virtual double calculatePay() const = 0;
};
class HourlyEmployee : public Employee {
public:
    double calculatePay() const override { /* hourly logic */ return 0; }
};
// The only switch lives in one factory:
std::unique_ptr<Employee> makeEmployee(const Record& r);
```

## 25. Use Descriptive Names for Functions
**Rule:** A long, descriptive name is better than a short, cryptic one. Use the name to describe the behavior.

**Bad:**
```cpp
void proc(int id);
void do1(Order o);
```

**Good:**
```cpp
void sendWelcomeEmail(int userId);
void applyDiscountToOrder(Order& order);
```

## 26. Prefer Few Function Arguments
**Rule:** Zero arguments is best, then one, then two. More than three should be avoided.

**Bad:**
```cpp
Circle makeCircle(double x, double y, double radius, Color color, bool filled);
```

**Good:**
```cpp
Circle makeCircle(Point center, double radius);
```

## 27. Avoid Flag Arguments
**Rule:** A boolean argument means the function does two things. Split it into two functions.

**Bad:**
```cpp
void render(bool isSuite);   // what does render(true) mean?
```

**Good:**
```cpp
void renderForSuite();
void renderForSingleTest();
```

## 28. Use Consistent Monadic Forms
**Rule:** A single-argument function should either ask a question about its argument, or transform it and return the result. Do not transform through an output argument.

**Bad:**
```cpp
void includeSetupPageInto(std::string& pageText);   // transforms via output argument
```

**Good:**
```cpp
bool fileExists(const std::string& name);            // question
std::string includeSetupPage(std::string pageText);  // transformation returned
```

## 29. Be Wary of Dyadic Functions
**Rule:** Two arguments are harder to read than one, and we may ignore the first one by mistake. Convert them to a monad when you can.

**Bad:**
```cpp
writeField(outputStream, name);
```

**Good:**
```cpp
outputStream.writeField(name);   // the stream becomes the receiver
```

## 30. Avoid Triads
**Rule:** Three arguments are much harder to understand than two. Reduce them or group them.

**Bad:**
```cpp
assertEquals(message, expected, actual);   // people often mix up the order
```

**Good:**
```cpp
assertExpectedEqualsActual(expected, actual);
```

## 31. Group Related Arguments into Objects
**Rule:** When several arguments form a concept, wrap them in a class that gives the concept a name.

**Bad:**
```cpp
void setRange(Date start, Date end, bool startInclusive, bool endInclusive);
```

**Good:**
```cpp
struct DateRange {
    Date start;
    Date end;
    bool startInclusive;
    bool endInclusive;
};
void setRange(const DateRange& range);
```

## 32. Use Keyword-Style Names
**Rule:** Encode the argument names in the function name, so the call is self-explanatory.

**Bad:**
```cpp
Complex fulcrum(23.0);   // what does 23.0 mean? real? imaginary?
```

**Good:**
```cpp
Complex fulcrum = Complex::FromRealNumber(23.0);
```

## 33. Functions Should Have No Side Effects
**Rule:** A function should do only what its name says. Hidden changes create surprises and temporal coupling.

**Bad:**
```cpp
bool checkPassword(const std::string& user, const std::string& pass) {
    if (valid(user, pass)) {
        Session::initialize();   // hidden side effect: erases session!
        return true;
    }
    return false;
}
```

**Good:**
```cpp
bool checkPassword(const std::string& user, const std::string& pass) {
    return valid(user, pass);
}
// Caller decides explicitly:
if (checkPassword(user, pass)) Session::initialize();
```

## 34. Avoid Output Arguments
**Rule:** Output arguments confuse readers. Modify the state of the object the method belongs to.

**Bad:**
```cpp
void appendFooter(std::string& report);
appendFooter(s);   // is s input or output?
```

**Good:**
```cpp
class Report {
public:
    void appendFooter();
};
report.appendFooter();
```

## 35. Separate Commands from Queries
**Rule:** A function either changes state or answers a question, not both.

**Bad:**
```cpp
if (set("username", "unclebob")) { ... }   // did it set, or was it already set?
```

**Good:**
```cpp
if (attributeExists("username")) {
    setAttribute("username", "unclebob");
}
```

## 36. Prefer Exceptions to Error Codes
**Rule:** Error codes force the caller to check immediately and create deep nesting. Exceptions separate the happy path from error handling.

**Bad:**
```cpp
if (deletePage(page) == E_OK) {
    if (registry.deleteReference(page.name) == E_OK) {
        // deeply nested...
    } else { log("registry failed"); }
} else { return E_ERROR; }
```

**Good:**
```cpp
try {
    deletePage(page);
    registry.deleteReference(page.name);
} catch (const std::exception& e) {
    logger.log(e.what());
}
```

## 37. Extract the Bodies of try and catch
**Rule:** Put the code inside `try` and `catch` into their own functions, so the error handling is one thing.

**Bad:**
```cpp
void deletePage(Page& page) {
    try {
        deletePageFromStore(page);
        registry.deleteReference(page.name);
    } catch (const std::exception& e) {
        logger.log(e.what());
    }
}
```

**Good:**
```cpp
void deletePage(Page& page) {
    try {
        deletePageAndAllReferences(page);
    } catch (const std::exception& e) {
        logError(e);
    }
}
void deletePageAndAllReferences(Page& page);   // no error handling here
void logError(const std::exception& e);
```

## 38. Don't Repeat Yourself (DRY)
**Rule:** Duplication is the root of many problems. Extract the repeated algorithm into one function.

**Bad:**
```cpp
if (setupPage) { out += "!include -setup ." + setupPath + "\n"; }
if (suiteSetup) { out += "!include -setup ." + suitePath + "\n"; }
if (teardown) { out += "!include -teardown ." + tearPath + "\n"; }
```

**Good:**
```cpp
void include(std::string& out, const std::string& pageName, const std::string& arg) {
    out += "!include " + arg + " ." + pageName + "\n";
}
include(out, setupPath, "-setup");
include(out, teardownPath, "-teardown");
```

## 39. Write Functions Messy First, Then Refine
**Rule:** Your first draft will be long and ugly. Write it, cover it with tests, then split and rename until it reads cleanly.

**Bad (first draft, never refined):**
```cpp
void process(const std::vector<int>& v) {
    int a = 0; int b = 0;
    for (size_t i = 0; i < v.size(); i++) { if (v[i] > 0) a += v[i]; else b += v[i]; }
    std::cout << a << " " << b << "\n";
}
```

**Good (refined under test coverage):**
```cpp
int sumPositive(const std::vector<int>& v);
int sumNegative(const std::vector<int>& v);
void printTotals(const std::vector<int>& v) {
    std::cout << sumPositive(v) << " " << sumNegative(v) << "\n";
}
```

---

# Part 3: Comments (Chapter 4)

## 40. Don't Comment Bad Code: Rewrite It
**Rule:** When code needs a comment to be understood, rewrite the code instead.

**Bad:**
```cpp
// Loop over the items, skip every other one, and add the odd ones up
int s = 0;
for (int i = 0; i < n; i += 2) s += v[i+1] ? v[i+1] : v[i];
```

**Good:**
```cpp
int sumOfOddPositionedItems(const std::vector<int>& items) {
    int sum = 0;
    for (std::size_t i = 0; i < items.size(); i += 2)
        sum += items[i];
    return sum;
}
```

## 41. Explain Yourself in Code
**Rule:** Most comments can be replaced by a well-named function or variable.

**Bad:**
```cpp
// Check to see if the employee is eligible for full benefits
if ((employee.flags & HOURLY_FLAG) && (employee.age > 65)) { ... }
```

**Good:**
```cpp
if (employee.isEligibleForFullBenefits()) { ... }
```

## 42. Comment the Intent, Not the Obvious
**Rule:** A good comment explains *why* a decision was made, when the code cannot show it.

**Bad:**
```cpp
i++; // increment i
```

**Good:**
```cpp
// Pages must sort after all other widget types so that
// the renderer emits headers before body content.
return 1;
```

## 43. Keep Comments Accurate
**Rule:** An inaccurate comment is worse than no comment. Update or delete comments when the code changes.

**Bad:**
```cpp
// Returns when closed is true.
void waitForClose(int timeoutMs) {       // actually throws after the timeout
    if (!closed) { wait(timeoutMs); if (!closed) throw std::runtime_error("timeout"); }
}
```

**Good:**
```cpp
// Blocks until closed becomes true, or throws if timeoutMs elapses first.
void waitForClose(int timeoutMs);
```

## 44. Don't Use Mandated Comments
**Rule:** Do not require a comment for every function or variable. Such comments add clutter and lies.

**Bad:**
```cpp
/**
 * @param title the title
 * @param author the author
 * @return void
 */
void addCD(const std::string& title, const std::string& author);
```

**Good:**
```cpp
void addCD(const std::string& title, const std::string& author);
```

## 45. Don't Keep Journal Comments
**Rule:** Source control already records who changed what and when. Remove history from the file.

**Bad:**
```cpp
// 2003-05-01  Bob: fixed off-by-one
// 2003-06-12  Alice: added caching
```

**Good:**
```cpp
// (no history in the file; use git log)
```

## 46. Avoid Noise and Redundant Comments
**Rule:** A comment that repeats the code adds nothing and is ignored by readers over time.

**Bad:**
```cpp
/** The day of the month. */
int dayOfMonth;

/** Default constructor. */
Foo() {}
```

**Good:**
```cpp
int dayOfMonth;
Foo() = default;
```

## 47. Delete Commented-Out Code
**Rule:** Commented-out code is dead code that nobody dares to delete. Version control keeps it safe.

**Bad:**
```cpp
response.setBody(formatter.resultStream());
// auto stream = formatter.getResultStream();
// response.setContent(read(stream));
```

**Good:**
```cpp
response.setBody(formatter.resultStream());
```

## 48. Avoid Position Markers and Closing-Brace Comments
**Rule:** Banners and `} // end if` comments are clutter. Make functions small so braces are visible.

**Bad:**
```cpp
// Actions ///////////////////////////////
void open();
void close();
} // end of class
```

**Good:**
```cpp
void open();
void close();
};
```

## 49. Keep Comments Local, Not System-Wide
**Rule:** A comment should describe the code it is next to, not some far-away part of the system.

**Bad:**
```cpp
/** Port on which the server runs. Defaults to 8082. */   // the real default is elsewhere
void setPort(int port);
```

**Good:**
```cpp
constexpr int kDefaultPort = 8082;   // defined next to its use
void setPort(int port);
```

---

# Part 4: Formatting (Chapter 5)

## 50. Use One Consistent Team Format
**Rule:** The whole team agrees on one style (braces, indentation, naming). Use an automatic formatter such as `clang-format`.

**Bad:**
```cpp
void f(){if(x){
 y();}
  else
{z();}}
```

**Good:**
```cpp
void f() {
    if (x) {
        y();
    } else {
        z();
    }
}
```

## 51. Keep Files Small
**Rule:** Most source files should be around 200 lines and rarely exceed 500. Small files are easier to understand.

**Bad:**
```text
engine.cpp   (6000 lines: parser, lexer, runtime, logging, I/O)
```

**Good:**
```text
lexer.cpp    (180 lines)
parser.cpp   (220 lines)
runtime.cpp  (150 lines)
logging.cpp  (60 lines)
```

## 52. Use the Newspaper Metaphor
**Rule:** A file reads like an article: the headline (class name) first, then the summary (public API), then the details.

**Bad:**
```cpp
// helpers first, the important part last
static void parseHeader();
static void parseBody();
class Document { public: void parse(); };
```

**Good:**
```cpp
class Document {          // headline
public:
    void parse();         // summary
private:
    void parseHeader();   // details
    void parseBody();
};
```

## 53. Separate Concepts with Vertical Blank Lines
**Rule:** Put blank lines between functions and between groups of statements that express different thoughts.

**Bad:**
```cpp
void a() { x(); }
void b() { y(); }
void c() { z(); }
```

**Good:**
```cpp
void a() { x(); }

void b() { y(); }

void c() { z(); }
```

## 54. Keep Related Code Vertically Dense
**Rule:** Lines that are closely related should be next to each other, without comments or blank lines between them.

**Bad:**
```cpp
int width;

// the height of the window
int height;
```

**Good:**
```cpp
int width;
int height;
```

## 55. Declare Variables Close to Their Use
**Rule:** Declare each local variable just before its first use, not at the top of a long function.

**Bad:**
```cpp
int total = 0;
int count = 0;
// 40 lines of other code
for (const auto& item : items) { total += item.price; ++count; }
```

**Good:**
```cpp
// 40 lines of other code
int total = 0;
for (const auto& item : items) total += item.price;
```

## 56. Put Instance Variables at the Top of the Class
**Rule:** Declare member variables in one well-known place, usually at the top of the class.

**Bad:**
```cpp
class Session {
public:
    void open();
private:
    int retries;   // hidden at the bottom
};
```

**Good:**
```cpp
class Session {
private:
    int retries = 0;
public:
    void open();
};
```

## 57. Put Callers Above Callees
**Rule:** When a function calls another, place the caller above the callee so the flow reads downward.

**Bad:**
```cpp
void helperA();   // callee at top
void helperB();
void mainEntry() { helperA(); helperB(); }
```

**Good:**
```cpp
void mainEntry() { helperA(); helperB(); }   // caller first
void helperA();
void helperB();
```

## 58. Keep Lines Under About 120 Characters
**Rule:** Lines should be short enough to read without scrolling sideways. Break long expressions.

**Bad:**
```cpp
auto result = computeSomethingVeryLong(argumentNumberOne, argumentNumberTwo, argumentNumberThree, argumentNumberFour);
```

**Good:**
```cpp
auto result = computeSomethingVeryLong(argumentNumberOne,
                                       argumentNumberTwo,
                                       argumentNumberThree,
                                       argumentNumberFour);
```

## 59. Use Horizontal Space to Show Precedence
**Rule:** Put spaces around low-precedence operators and none around high-precedence ones, so the formula is readable.

**Bad:**
```cpp
double r = (-b+std::sqrt(d))/(2*a);
```

**Good:**
```cpp
double r = (-b + std::sqrt(d)) / (2*a);
```

## 60. Indent Scopes; Don't Collapse Them
**Rule:** Always indent the body of a block. Do not write one-line blocks, because they hide the structure.

**Bad:**
```cpp
class CommentWidget { public: std::string render() const {return "";} };
```

**Good:**
```cpp
class CommentWidget {
public:
    std::string render() const {
        return "";
    }
};
```

---

# Part 5: Objects and Data Structures (Chapter 6)

## 61. Hide Data Behind Abstractions
**Rule:** Do not expose the internal representation of a value. Expose operations that express the abstract idea.

**Bad:**
```cpp
struct Vehicle {
    double fuelTankCapacityGallons;
    double gallonsOfGasoline;   // callers must do the math
};
```

**Good:**
```cpp
class Vehicle {
public:
    double percentFuelRemaining() const;   // no details exposed
private:
    double gallonsOfGasoline_;
    double tankCapacity_;
};
```

## 62. Don't Create Hybrids of Objects and Data Structures
**Rule:** A class should either hide data and expose behavior, or be a plain data structure. Half-and-half classes are the worst of both.

**Bad:**
```cpp
class Account {
public:
    double balance;             // public data
    void deposit(double amt);   // and behavior: hybrid, hard to extend
};
```

**Good:**
```cpp
class Account {                 // object: private data, behavior
public:
    void deposit(Money amount);
private:
    Money balance_;
};
```

## 63. Obey the Law of Demeter (Talk to Friends, Not Strangers)
**Rule:** A method should call only its own members, its arguments, objects it creates, and objects it holds directly.

**Bad:**
```cpp
auto path = ctxt.getOptions().getScratchDir().getAbsolutePath();
```

**Good:**
```cpp
auto path = ctxt.scratchDirectoryPath();
```

## 64. Avoid Train Wrecks
**Rule:** Long chains of calls couple the caller to the whole structure. Split them or hide them.

**Bad:**
```cpp
order.getCustomer().getAddress().getCity().toUpper();
```

**Good:**
```cpp
std::string city = order.shippingCity();   // Order knows how to find it
```

## 65. Don't Add Getters and Setters Blindly
**Rule:** Adding a getter and setter for every field exposes the internals. Model behavior instead.

**Bad:**
```cpp
class Account {
public:
    double getBalance() const;
    void setBalance(double b);   // anyone can change any value
};
```

**Good:**
```cpp
class Account {
public:
    void deposit(Money amount);
    bool withdraw(Money amount);   // enforces the rules
    Money balance() const;
};
```

---

# Part 6: Error Handling (Chapter 7)

## 66. Write the try-catch Block First
**Rule:** When code may throw, start with the `try-catch` scope and a test that expects the exception. Then fill in the logic.

**Bad:**
```cpp
std::vector<Record> retrieveSection(const std::string& name) {
    std::ifstream f(name);              // error handling added as an afterthought
    return readAll(f);
}
```

**Good:**
```cpp
std::vector<Record> retrieveSection(const std::string& name) {
    try {
        std::ifstream f(name);
        if (!f) throw std::runtime_error("cannot open");
        return readAll(f);
    } catch (const std::runtime_error& e) {
        throw StorageError("retrieval error", e);
    }
}
```

## 67. Provide Context with Exceptions
**Rule:** An exception message should say what operation failed and why, so logs are useful.

**Bad:**
```cpp
throw std::runtime_error("error");
```

**Good:**
```cpp
throw StorageError("failed to open section file: " + name);
```

## 68. Define Exception Classes by the Caller's Needs
**Rule:** Classify errors by how the caller will handle them, not by their source. Often one exception type is enough.

**Bad:**
```cpp
try { port.open(); }
catch (const DeviceResponseError& e) { report(e); }
catch (const UnlockError& e)         { report(e); }
catch (const GmxError& e)            { report(e); }
```

**Good:**
```cpp
try { port.open(); }
catch (const PortDeviceFailure& e) { report(e); }
```

## 69. Wrap Third-Party APIs
**Rule:** Wrap vendor code in your own class. Translate its exceptions and return types to your own, so you can change vendors and mock easily.

**Bad:**
```cpp
ACMEPort port(12);
port.open();   // vendor exceptions leak everywhere
```

**Good:**
```cpp
class LocalPort {
public:
    explicit LocalPort(int number) : inner_(number) {}
    void open() {
        try { inner_.open(); }
        catch (const std::exception& e) { throw PortDeviceFailure(e.what()); }
    }
private:
    ACMEPort inner_;
};
```

## 70. Define the Normal Flow (Special Case Object)
**Rule:** Instead of catching an exception for a special case, return an object that handles that case.

**Bad:**
```cpp
int total = 0;
try {
    total += expenseReport.mealsFor(employee).total();
} catch (const MealsNotFound&) {
    total += perDiemAmount();
}
```

**Good:**
```cpp
// mealsFor returns PerDiemMeals when there are no receipts.
int total = expenseReport.mealsFor(employee).total();
```

## 71. Don't Return Null
**Rule:** Returning null pushes checks onto every caller. Return an empty collection, `std::optional`, or a special-case object.

**Bad:**
```cpp
Employee* findEmployee(int id);   // may return nullptr
Employee* e = findEmployee(42);
if (e != nullptr) e->pay();
```

**Good:**
```cpp
std::optional<Employee> findEmployee(int id);
std::vector<Employee> employeesInDepartment(int dept);   // empty, never null
```

## 72. Don't Pass Null
**Rule:** Do not pass null as an argument unless the API requires it. Use references, which cannot be null.

**Bad:**
```cpp
double xProjection(const Point* p1, const Point* p2) {
    return (p2->x - p1->x) * 1.5;   // crashes if null
}
calc.xProjection(nullptr, &p);
```

**Good:**
```cpp
double xProjection(const Point& p1, const Point& p2) {
    return (p2.x - p1.x) * 1.5;
}
```

---

# Part 7: Boundaries (Chapter 8)

## 73. Don't Pass Third-Party Interfaces Around Your System
**Rule:** Keep a broad interface like `std::map` inside a class that offers a narrow, meaningful API.

**Bad:**
```cpp
std::map<std::string, Sensor*> sensors;   // passed to many functions
void report(const std::map<std::string, Sensor*>& sensors);
```

**Good:**
```cpp
class Sensors {
public:
    Sensor* getById(const std::string& id) const;
    void add(const std::string& id, Sensor* s);
private:
    std::map<std::string, Sensor*> sensors_;   // boundary hidden here
};
```

## 74. Write Learning Tests
**Rule:** Before using a third-party library, write small tests that show how it really behaves. They document your understanding and warn you when the library changes.

**Bad:**
```cpp
// Use the library in production code and debug when it fails.
logger.addAppender(makeAppender());
```

**Good:**
```cpp
TEST(LibraryLearning, RegexMatchesHttpDateFormat) {
    std::regex re(R"(\w{3}, \d{2} \w{3} \d{4} \d{2}:\d{2}:\d{2} GMT)");
    EXPECT_TRUE(std::regex_match("Tue, 02 Apr 2003 22:18:49 GMT", re));
}
```

## 75. Code Against the Interface You Wish You Had
**Rule:** When the other side is not ready, define your own interface the way you want it, and write an adapter later.

**Bad:**
```cpp
// Coupled to a vendor API that does not exist yet
vendor::XmitHw::keyOn(freqHz);
vendor::XmitHw::sendStream(stream);
```

**Good:**
```cpp
class Transmitter {               // the interface we wish we had
public:
    virtual ~Transmitter() = default;
    virtual void transmit(double frequency, const Stream& data) = 0;
};

class VendorTransmitterAdapter : public Transmitter {   // written later
public:
    void transmit(double f, const Stream& d) override { vendor::send(f, d); }
};
```

---

# Part 8: Unit Tests (Chapter 9)

## 76. Keep Tests as Clean as Production Code
**Rule:** Test code is code. Dirty tests become a burden, and eventually people delete them.

**Bad:**
```cpp
TEST(Pages, t1) {
    auto p = mk(a, "PageOne"); auto q = mk(a, "PageOne.ChildOne");
    auto r = resp(root, PathParser::parse("root"), "type:pages");
    assert(r.find("<name>PageOne</name>") != npos && r.find("ChildOne") != npos);
}
```

**Good:**
```cpp
TEST(PageHierarchy, ListsAllPagesAsXml) {
    makePages("PageOne", "PageOne.ChildOne", "PageTwo");
    auto response = submitRequest("root", "type:pages");
    EXPECT_TRUE(response.isXml());
    EXPECT_TRUE(response.contains("<name>ChildOne</name>"));
}
```

## 77. Tests Enable Change (the "-ilities")
**Rule:** With a good test suite you can change the design safely. Without tests, every change is a risk.

**Bad:**
```cpp
// No tests: the team avoids touching this code.
double computeTax(double amount) { return amount * 0.09 + legacyRounding(amount); }
```

**Good:**
```cpp
TEST(Tax, ComputesNinepercent) { EXPECT_NEAR(computeTax(100.0), 9.0, 1e-9); }
// With this test, we can safely refactor computeTax later.
```

## 78. Use BUILD-OPERATE-CHECK
**Rule:** Structure each test in three parts: build the test data, operate on it, and check the result.

**Bad:**
```cpp
TEST(Account, Withdraw) {
    Account a(Money(100)); a.withdraw(Money(30));
    EXPECT_EQ(a.balance().cents(), 7000);
    Account b(Money(10)); EXPECT_TRUE(b.withdraw(Money(5)));
}
```

**Good:**
```cpp
TEST(Account, WithdrawReducesBalance) {
    Account account(Money(100));          // BUILD
    account.withdraw(Money(30));          // OPERATE
    EXPECT_EQ(account.balance(), Money(70)); // CHECK
}
```

## 79. Build a Domain-Specific Testing Language
**Rule:** Create helper functions that express the test in the vocabulary of the problem, so tests read like sentences.

**Bad:**
```cpp
hw.setTemperature(-40);
controller.tic();
EXPECT_TRUE(hw.heaterState());
EXPECT_TRUE(hw.blowerState());
EXPECT_FALSE(hw.coolerState());
```

**Good:**
```cpp
wayTooCold();                     // helper that hides the details
EXPECT_EQ(hw.state(), "HBchL");   // compact state string
```

## 80. Test One Concept per Test
**Rule:** Each test should check one concept. Use multiple asserts only when they check that same concept.

**Bad:**
```cpp
TEST(Dates, AddMonths) {
    EXPECT_EQ(addMonths(1, may31), june30);     // concept 1: month-end clamp
    EXPECT_EQ(addMonths(2, may31), july31);     // concept 2: normal add
    EXPECT_EQ(addMonths(1, addMonths(1, may31)), july30); // concept 3
}
```

**Good:**
```cpp
TEST(Dates, ClampsToEndOfShorterMonth) { EXPECT_EQ(addMonths(1, may31), june30); }
TEST(Dates, AddsTwoMonthsNormally)     { EXPECT_EQ(addMonths(2, may31), july31); }
```

## 81. Follow F.I.R.S.T. (Fast, Independent, Repeatable, Self-validating, Timely)
**Rule:** Tests should be fast, independent of each other, repeatable in any environment, self-checking (pass or fail), and written in time.

**Bad:**
```cpp
TEST(Cache, Stores) { cache.put("a", 1); }                   // leaves state behind
TEST(Cache, Reads)  { EXPECT_EQ(cache.get("a"), 1); }        // depends on the previous test
TEST(Clock, Waits)  { std::this_thread::sleep_for(60s); }    // slow
```

**Good:**
```cpp
TEST(Cache, StoresAndReadsValue) {
    Cache cache;                 // fresh state for each test
    cache.put("a", 1);
    EXPECT_EQ(cache.get("a"), 1);
}
TEST(Clock, TimeoutFiresWithFakeClock) {
    FakeClock clock;             // no real waiting: fast and repeatable
    EXPECT_TRUE(clock.advanceAndCheckTimeout(60));
}
```

---

# Part 9: Classes (Chapter 10)

## 82. Classes Should Be Small
**Rule:** Measure classes by the number of responsibilities, not by lines. A class with a long name or many "and" words has too many.

**Bad:**
```cpp
class SuperDashboard : public QFrame {
public:
    std::string customizerLanguagePath();
    int majorVersion(); int minorVersion(); int buildNumber();
    void showObject(MetaObject*);
    void addProject(Project*);
    // ... 70 more methods
};
```

**Good:**
```cpp
class Version {
public:
    int major() const; int minor() const; int build() const;
};
class ProjectManager { /* project methods */ };
```

## 83. Follow the Single Responsibility Principle (SRP)
**Rule:** A class should have one reason to change.

**Bad:**
```cpp
class Report {
public:
    void calculateTotals();   // reason 1: business rules change
    void printToHtml();       // reason 2: presentation changes
    void saveToDatabase();    // reason 3: storage changes
};
```

**Good:**
```cpp
class Report { public: void calculateTotals(); };
class HtmlReportPrinter { public: void print(const Report&); };
class ReportRepository { public: void save(const Report&); };
```

## 84. Maintain High Cohesion
**Rule:** Methods of a class should use the class's variables. A class where each method uses most variables is highly cohesive.

**Bad:**
```cpp
class Thing {
    int count_;
    std::string label_;
public:
    int size() const { return count_; }      // uses count_ only
    std::string name() const { return label_; } // uses label_ only
};   // two unrelated groups in one class
```

**Good:**
```cpp
class Stack {
    int topOfStack_ = 0;
    std::vector<int> elements_;
public:
    int size() const { return topOfStack_; }
    void push(int e) { ++topOfStack_; elements_.push_back(e); }
    int pop() { int e = elements_.back(); elements_.pop_back(); --topOfStack_; return e; }
};
```

## 85. Split Classes When Cohesion Drops
**Rule:** When extracting functions forces you to add many instance variables used by only a few methods, extract a new class.

**Bad:**
```cpp
class PrimePrinter {
    int rowsPerPage_, columnsPerPage_, pageNumber_, pageOffset_;   // used by subsets only
public:
    void printPrimes();
    void printPage();      // uses only rows/columns
    void generatePrimes(); // uses none of the above
};
```

**Good:**
```cpp
class RowColumnPagePrinter { int rowsPerPage_; int columnsPerPage_; public: void print(...); };
class PrimeGenerator { public: static std::vector<int> generate(int n); };
```

## 86. Follow the Open-Closed Principle (OCP)
**Rule:** Classes should be open for extension and closed for modification. Add new behavior with new subclasses instead of editing old code.

**Bad:**
```cpp
class Sql {
public:
    std::string generate() {
        if (type_ == "create") return createSql();
        if (type_ == "select") return selectSql();
        // every new statement type edits this method
    }
};
```

**Good:**
```cpp
class Sql { public: virtual std::string generate() const = 0; virtual ~Sql() = default; };
class CreateSql : public Sql { public: std::string generate() const override; };
class SelectSql : public Sql { public: std::string generate() const override; };
class UpdateSql : public Sql { public: std::string generate() const override; }; // added, nothing else changed
```

## 87. Depend on Abstractions (Dependency Inversion Principle)
**Rule:** High-level classes should depend on interfaces, not on concrete low-level details.

**Bad:**
```cpp
class Portfolio {
    TokyoStockExchange exchange_;   // concrete, hard to test
public:
    Money value() const { return exchange_.currentPrice("MSFT") * 5; }
};
```

**Good:**
```cpp
class StockExchange {
public:
    virtual ~StockExchange() = default;
    virtual Money currentPrice(const std::string& symbol) const = 0;
};
class Portfolio {
    const StockExchange& exchange_;   // injected abstraction
public:
    explicit Portfolio(const StockExchange& e) : exchange_(e) {}
    Money value() const { return exchange_.currentPrice("MSFT") * 5; }
};
```

## 88. Keep Variables Private (Encapsulation)
**Rule:** Keep data private. Make something protected or package-visible only as a last resort, for example for tests.

**Bad:**
```cpp
class Counter {
public:
    int count;   // anyone can change it
};
```

**Good:**
```cpp
class Counter {
public:
    int count() const { return count_; }
    void increment() { ++count_; }
private:
    int count_ = 0;
};
```

---

# Part 10: Systems (Chapter 11)

## 89. Separate Construction from Use
**Rule:** Building objects and their dependencies is one job. Using them is another. Do not mix them in the same function.

**Bad:**
```cpp
Service& getService() {
    static MyServiceImpl* svc = nullptr;
    if (!svc) svc = new MyServiceImpl(loadConfig(), openDb());  // construction hidden in use
    return *svc;
}
```

**Good:**
```cpp
int main() {                                    // construction happens here only
    auto config = loadConfig();
    auto db = openDb(config);
    auto service = std::make_shared<MyServiceImpl>(config, db);
    Application app(service);                  // application just uses it
    app.run();
}
```

## 90. Use Dependency Injection
**Rule:** An object should receive its dependencies through its constructor or setters, instead of creating them.

**Bad:**
```cpp
class OrderProcessor {
    MySqlDatabase db_;   // hard-wired
public:
    void process(const Order& o) { db_.save(o); }
};
```

**Good:**
```cpp
class OrderProcessor {
    std::shared_ptr<Database> db_;   // injected abstraction
public:
    explicit OrderProcessor(std::shared_ptr<Database> db) : db_(std::move(db)) {}
    void process(const Order& o) { db_->save(o); }
};
```

## 91. Use Standards Only When They Add Real Value
**Rule:** Do not add heavy frameworks or patterns just because they are standard. Use the simplest thing that works.

**Bad:**
```cpp
// A full plugin registry for a single hard-coded operation
class OperationRegistry { /* 300 lines of factories and reflection */ };
```

**Good:**
```cpp
int add(int a, int b) { return a + b; }   // the simplest thing that works today
```

## 92. Postpone Decisions to the Last Responsible Moment
**Rule:** Do not decide on the database, framework, or architecture before you need to. Decide when you have the best information, while keeping the design decoupled.

**Bad:**
```cpp
// Chosen on day one, before any requirements exist
using Storage = PostgresStorage;   // spread everywhere
```

**Good:**
```cpp
class Storage {                    // interface kept open
public:
    virtual ~Storage() = default;
    virtual void save(const Record&) = 0;
};
// The concrete choice is made later, in one place (main).
```

---

# Part 11: Emergence (Chapter 12)

## 93. Simple Design Rule 1: Run All the Tests
**Rule:** A design must produce a system that passes all its tests. A design you cannot verify is not a design you can trust.

**Bad:**
```cpp
// Tests were never written; nobody knows whether this works.
void calculateInvoice(Invoice& inv) { /* ... */ }
```

**Good:**
```cpp
TEST(Invoice, TotalIncludesTax) {
    Invoice inv = makeInvoice(100.0);
    calculateInvoice(inv);
    EXPECT_NEAR(inv.total(), 109.0, 1e-9);
}
```

## 94. Simple Design Rule 2: No Duplication
**Rule:** Remove duplicated code. Duplication is a missed abstraction.

**Bad:**
```cpp
void scaleToOneDimension(...) { /* ... */ image.dispose(); std::gc(); image = newImage; }
void rotate(int deg)         { /* ... */ image.dispose(); std::gc(); image = newImage; }
```

**Good:**
```cpp
void replaceImage(Image newImage) { image.dispose(); std::gc(); image = newImage; }
void scaleToOneDimension(...) { /* ... */ replaceImage(scaled); }
void rotate(int deg)         { replaceImage(rotated(image, deg)); }
```

## 95. Simple Design Rule 3: Express Intent
**Rule:** Write code so that other programmers understand it quickly. Use good names, small functions, and standard patterns.

**Bad:**
```cpp
int f(int a) { return a * 2 + 1; }
```

**Good:**
```cpp
int totalPriceWithServiceFee(int basePrice) {
    constexpr int kDoubled = 2;
    constexpr int kServiceFee = 1;
    return basePrice * kDoubled + kServiceFee;
}
```

## 96. Simple Design Rule 4: Minimize Classes and Methods
**Rule:** Keep the number of classes and methods low, but never at the cost of the first three rules. Avoid pointless dogma such as an interface for every class.

**Bad:**
```cpp
class IPriceCalculator { public: virtual int price() const = 0; };
class PriceCalculator : public IPriceCalculator { public: int price() const override { return 5; } };
```

**Good:**
```cpp
class PriceCalculator { public: int price() const { return 5; } };   // only abstract when needed
```

---

# Part 12: Concurrency (Chapter 13)

## 97. Keep Concurrency Code Separate
**Rule:** Thread management is a separate responsibility. Keep it out of business logic, so it can be tested and changed alone.

**Bad:**
```cpp
void handleRequest(Request r) {
    std::thread([r]() {
        // business logic mixed with thread creation
        process(r);
    }).detach();
}
```

**Good:**
```cpp
class RequestScheduler {                 // only thread policy here
public:
    void schedule(std::function<void()> task) { pool_.submit(std::move(task)); }
private:
    ThreadPool pool_;
};
void handleRequest(Request r) { process(r); }   // business logic, no threads
```

## 98. Limit the Scope of Shared Data
**Rule:** Share as little data as possible. Prefer copies and local results merged at the end.

**Bad:**
```cpp
std::vector<Result> shared;   // many threads push_back without a lock: race condition
void worker(int i) { shared.push_back(compute(i)); }
```

**Good:**
```cpp
void worker(int i, std::vector<Result>& local) { local.push_back(compute(i)); }
// Each thread writes its own vector; the main thread merges them after join().
```

## 99. Keep Synchronized Sections Small
**Rule:** Hold a lock only around the shared data access. Do not hold locks during slow work such as I/O.

**Bad:**
```cpp
std::lock_guard<std::mutex> lock(mutex_);
auto page = downloadPage(url);   // slow I/O while locked
pages_.push_back(page);
```

**Good:**
```cpp
auto page = downloadPage(url);   // unlocked: runs in parallel
{
    std::lock_guard<std::mutex> lock(mutex_);
    pages_.push_back(page);      // lock only for the shared update
}
```

## 100. Don't Ignore One-Off Failures; Make Threaded Code Testable
**Rule:** A rare failure in threaded code is a real bug. Expose it by running tests many times, on many platforms, with more threads than cores, and with injected delays.

**Bad:**
```cpp
TEST(Counter, Increments) {
    Counter c;
    std::thread t1([&]{ c.increment(); });
    std::thread t2([&]{ c.increment(); });
    t1.join(); t2.join();
    EXPECT_EQ(c.value(), 2);   // passes most of the time; the bug hides
}
```

**Good:**
```cpp
TEST(Counter, IncrementsUnderContention) {
    for (int run = 0; run < 100000; ++run) {       // stress: run many times
        Counter c;                                 // thread-safe Counter
        std::thread t1([&]{ c.increment(); });
        std::thread t2([&]{ std::this_thread::yield(); c.increment(); });  // forced interleaving
        t1.join(); t2.join();
        ASSERT_EQ(c.value(), 2) << "failed on run " << run;
    }
}
```

---

## Summary Checklist

- Names reveal intent, and are pronounceable and searchable.
- Functions are small, do one thing, and sit at one level of abstraction.
- Comments are rare; code explains itself.
- Formatting is consistent and the reader can scan it.
- Objects hide data; data structures expose data; never mix.
- Errors use exceptions, never null, and never leak vendor details.
- Boundaries are wrapped and tested with learning tests.
- Tests are clean, fast, independent, and one concept each.
- Classes have one responsibility, high cohesion, and depend on abstractions.
- Systems separate construction from use; decisions are postponed until needed.
- Every change keeps the tests passing, and we leave the code cleaner than we found it (the Boy Scout Rule).
