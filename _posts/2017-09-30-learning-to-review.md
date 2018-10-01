## Code reviews, among other things, are a platform for team members to better each other.
And i love it. Working as a backend software engineer, I find it as one of the best tools to grow. Here's my mutable/improving process for reviewing. It's no law, but it's a start to something solid.

#### I usually do a multi pass review concentrating on different layers of abstraction.

### -File level-
- Import code under review to my IDE, so I can always play with it or break it.
- First see if the files I expect to see are there. From simple classes to their corresponding test to config. Can easily spot mistakes that make project architecture inconsistent.
    - Somebody checks in `Clazz` but no `ClazzTest`, easy to catch missing tests.
    - Somebody works on a feature requiring database changes. I expect to see a change in wherever the project keeps track of DB changes (a class, a DB migration file, etc).

### -Functions as black boxes-
- If the building blocks of the feature are seen as [input] -> [feature implementation] -> [output] I do a pass and try to fit the code under review in a black box pattern. This way I can make sure that all of the data going in and out is what I expect it to be, without being biased by implemetation details. "To build this, I'd need `x` going in and `y` coming out, nothing more nothing less!" is what I tell myself to answer this question.
- Then I try to challenge the design of the those black boxes in the hierarchy/sequence level. "Does block A need to be before block B?" for example.
- I look for documentation (or lack of) to help me with these questions in this step.

### -Digging in-
- Then I do another visual pass but now at the function level. I get all [Uncle Bob](https://en.wikipedia.org/wiki/Robert_C._Martin) and check for naming and readability. I read the method name and expect the returned value and its code to match that name. Nothing more, nothing less. Also I quickly think about how I would write it differently. If I manage to come up with a better alternative I comment on it, if not I focus on what makes the current implementation superior and take a mental note. 
- Then I read line by line and pay attention to variable naming, code inspection alerts (IDE, linters etc) and see if I can come up with more idiomatic solutions. This is a good step to check for performance or security (in separate passes), provided that the architecture of the solution under review is to your liking.

### -Tests-
- Then I go to tests, and give them almost the same readability passes as well. I start with the larger integration tests since they cover the most functionality. 
- I ask myself if what I'm seeing belongs here or in another test (a unit test for example) to avoid testing overlap.
- I look into the assertions and formulate assumptions of what this method is testing. Then I look at the test name and assertion error messages and expect them to be named accordingly.
- If coverage is your thing, now is the time to run the tool of your choice.
- Lastly, I check for test data initialization overlap and corectness.

If at any step of the above passes something looks fishy, I try and break the test.

### -Commenting-
I try to avoid using words like 'think' or 'feel'. I find it common courtesy for review comments to be well-thought proposals. I try to compose my comments as such:

- Why I disagree with the current implementation.
- How would I've done it differently (if possible provide some details in code).
- What's the benefit of my suggestion over the current implementation.

For example "This code is bad and unreadable" makes for a poor review comment. A better version of it would be "This method does not comply with the project style because x. The method naming could be clearer, for example method y should be named z or refactored into two separate methods. Also please add doc to every public method"

Lastly, keep in mind the context in which the person under review processes your input. Written words carry no tone and can be misinterpreted easily. Opt for neutrality and clarity.


### -Related reading-

Check out [this](https://sback.it/publications/icse2018seip.pdf) publication on the subject. Skip to chapters 5, 7 for a quick read.