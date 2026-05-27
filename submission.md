Homework 5 Submission

Links

* Repo (hw5 branch): [https://github.com/NU-CS-Software-Studio-Spring-26/homework-5-KenZheng96/tree/hw5](https://github.com/NU-CS-Software-Studio-Spring-26/homework-5-KenZheng96/tree/hw5)   
* .cursorignore: [https://github.com/NU-CS-Software-Studio-Spring-26/homework-5-KenZheng96/blob/main/.cursorignore](https://github.com/NU-CS-Software-Studio-Spring-26/homework-5-KenZheng96/blob/main/.cursorignore)   
* AGENTS.md: [https://github.com/NU-CS-Software-Studio-Spring-26/homework-5-KenZheng96/blob/main/AGENTS.md](https://github.com/NU-CS-Software-Studio-Spring-26/homework-5-KenZheng96/blob/main/AGENTS.md)   
* rails-conventions.mdc: [https://github.com/NU-CS-Software-Studio-Spring-26/homework-5-KenZheng96/blob/main/.cursor/rules/rails-conventions.mdc](https://github.com/NU-CS-Software-Studio-Spring-26/homework-5-KenZheng96/blob/main/.cursor/rules/rails-conventions.mdc)   
* security.mdc: [https://github.com/NU-CS-Software-Studio-Spring-26/homework-5-KenZheng96/blob/main/.cursor/rules/security.mdc](https://github.com/NU-CS-Software-Studio-Spring-26/homework-5-KenZheng96/blob/main/.cursor/rules/security.mdc)   
* Part 4 PR: [https://github.com/NU-CS-Software-Studio-Spring-26/nu-cs-software-studio-spring-26-homework-5-hw5/pull/10](https://github.com/NU-CS-Software-Studio-Spring-26/nu-cs-software-studio-spring-26-homework-5-hw5/pull/10) 

Part 3: Various-mode Prompting

Ask Mode:  
Prompt used:

* Where in this codebase is the todos index filtered or queried? Cite the exact files and line numbers. Do not propose changes.

Cursor response:  
\- app/controllers/todos\_controller.rb:6 — @todos \= Todo.all (loads all todos with no filter conditions)  
\- app/views/todos/index.html.erb:8 — Iterates over @todos with \<% @todos.each do |todo| %\>

Verification:

* I opened both files and confirmed the citations are real. Line 6 of todos\_controller.rb is indeed @todos \= Todo.all, and index.html.erb iterates over @todos at that line.

Plan Mode:  
Prompt used:

* I want to change the todos index so that only the user who created a todo can mark it done; other authenticated users can see it but cannot toggle it. Propose a plan as a numbered list of changes, including files to edit, new tests to add, and any migration. Do not write code.


Plan returned by Cursor (edited):

1\. Install and configure Devise authentication \- DEFERRED: app has no existing auth, would need a separate spike before this feature  
2\. Add Todo ownership and done status via migrations \- add todos.user\_id and todos.done boolean default false, not null  
3\. Add model associations \- belongs\_to :user on Todo, has\_many :todos on User, optional owned\_by? helper  
4\. Assign owner on create and add toggle\_done action in TodosController with server-side authorization  
5\. Add route for done toggling — member route for toggle\_done under resources :todos in config/routes.rb  
6\. Update Todo UI — show done status for all users, show toggle only for owners  
7\. Add controller/integration tests for auth and authorization

My edits to the plan:

* Removed steps 8-9 (system tests and fixture updates) as out of scope for this exercise. Deferred step 1 (Devise install) since the app has no existing auth. Choose step 5 as the Agent mode target since it is the smallest single change.

Agent Mode:  
Prompt used:

* In config/routes.rb, add a member route for toggle\_done under resources :todos. Do not touch any other file.

Commit:  
[https://github.com/NU-CS-Software-Studio-Spring-26/homework-5-KenZheng96/commit/a0e04f17452bb7012f3f2b71f2dc21f7788db30c](https://github.com/NU-CS-Software-Studio-Spring-26/homework-5-KenZheng96/commit/a0e04f17452bb7012f3f2b71f2dc21f7788db30c) 

Bad to Good Prompt Rewrite:

Bad prompt:

* fix the bug in todos

Good prompt:

* Context: app/models/todo.rb, app/controllers/todos\_controller.rb, test/controllers/todos\_controller\_test.rb  
    
* Task: Add a validation to Todo that requires description to be present and non-empty.  
    
* Expected vs actual: Submitting the new todo form with a blank description should fail with a validation error message. Currently it saves successfully and creates a todo with an empty description.  
    
* Constraints: Only touch app/models/todo.rb and test/controllers/todos\_controller\_test.rb. Do not add any new gems. Follow existing Minitest patterns in the test file.  
    
* Done when: bin/rails test test/controllers/todos\_controller\_test.rb passes and includes a test asserting a todo with blank description is invalid.

Part 4: Turbo Streams  
My Explanation of Turbo Streams:

Turbo Streams are a Rails feature that lets a controller send back a small HTML fragment to update a specific part of the page, instead of doing a full page reload. When a user clicks a button wired up with Turbo, the browser sends a request with:   
Accept: text/vnd.turbo-stream.html.   
If the controller responds with format.turbo\_stream, Rails renders a .turbo\_stream.erb view file and sends it back with:  
Content-Type: text/vnd.turbo-stream.html.   
The Turbo JS library intercepts the response and applies it to the DOM using one of seven actions: append, prepend, replace, update, remove, before, or after.

The controller pattern looks like:  
respond\_to do |format|  
  format.turbo\_stream  
  format.html { redirect\_to todos\_path }  
end

* The matching view file goes in app/views/todos/toggle\_priority.turbo\_stream.erb.

One thing I verified: 

* Cursor stated the MIME type is text/vnd.turbo-stream.html. I verified this against the Turbo Streams handbook at https://turbo.hotwired.dev/reference/streams which confirms this is the official content type for Turbo Stream responses.

Self-check answers:

* MIME type: text/vnd.turbo-stream.html  
* View file for toggle\_priority on TodosController: app/views/todos/toggle\_priority.turbo\_stream.erb

Part 4 PR:  
PR URL: [https://github.com/NU-CS-Software-Studio-Spring-26/nu-cs-software-studio-spring-26-homework-5-hw5/pull/10](https://github.com/NU-CS-Software-Studio-Spring-26/nu-cs-software-studio-spring-26-homework-5-hw5/pull/10)'   
