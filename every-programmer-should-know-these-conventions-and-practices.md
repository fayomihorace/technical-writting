# Every programmer should know these conventions and practices

Without rules and standards, we would be living in total chaos, and if every programmer had the freedom to do or write whatever he wanted as code, we would probably still be in the time where computers could occupy an entire apartment 😁.


![Image description](https://img.freepik.com/free-photo/sofware-developer-thinking-while-touching-beard-while-typing-laptop-sitting-desk-with-multiple-screens-parsing-code-focused-database-admin-working-with-team-coding-background_482257-33556.jpg)

But fortunately, this is not the case 😌. There are rules and standards every programmer should know. If you didn’t know these standards, don’t worry, I was a bad programmer too, a very bad one, but now I’m better and I’m still improving. Now, if you already knew them I congratulate you. But read anyway until the end for peace of mind.

I won’t really detail or give examples to these rules, as each of them requires at least a whole article with a lot of real cases examples, exceptions, or edge cases. I’ll just list them and cover each of them alone in its own article.


1. **Take care of your code quality**
It implies that you write readable code. Use styling rules related to your programming language or technology and be consistent about them (For instance in python, the coding style conventions are described in PEP 8 of the documentation). You can install a linting extension related to your programming language/technology in your IDE. It will enforce your code quality and give you good habits.


2. **Give meaningful names**
Give a meaningful name to your variables, functions, class, and packages. It will help understand the big picture of your 
codebase without even reading the implementation details.


3. **Use the DRY (Don't Repeat Yourself) principle**
Avoid as much as possible repeating your code. It will first reduce your codebase, and if for any reason you have to change or improve that code, you'll change it only once instead of applying the same changes at multiple places.


4. **Write short functions that handle specific tasks**
Keep your functions as short as possible and make them handle a single task. Plus, it will help for code reusability and code refactoring.


5. **Well document your code**
Add useful comments and document your functions, classes, and packages as much as possible. Add description and type-hint for function parameters and return values. But, avoid obvious comments. It will also help to understand the big picture of your codebase without even reading the implementation details. And some of your IDE extensions could even help you automatically generate a documentation for your software if it's well documented.


6. **Avoid using Magic numbers or hardcoded values in your code**
It's will save your life and contribute to your codebase maintainability.

7. **Limit code line length to a standard value (80 chars for instance)**
It improves code readability as well.
