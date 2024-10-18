[comment]: # (Compile this presentation with the command below)
[comment]: # (mdslides index.md && mv index/index.html .)
[comment]: # (THEME = night)
[comment]: # (CODE_THEME = base16/zenburn)
[comment]: # (The list of themes is at https://revealjs.com/themes/)
[comment]: # (The list of code themes is at https://highlightjs.org/)
[comment]: # (Pass optional settings to reveal.js:)
[comment]: # (controls: true)
[comment]: # (keyboard: true)
[comment]: # (progress: true)
[comment]: # (width: "1024")
[comment]: # (markdown: { smartypants: true })
[comment]: # (hash: false)
[comment]: # (respondToHashChanges: false)
[comment]: # (Other settings are documented at https://revealjs.com/config/)

### Thinking Like a Tester

----------

Kevin Buffardi, Ph.D.

Professor, California State University, Chico

[LearnByFailure.com](https://learnbyfailure.com/)

<img src="qr-code-thinking-like-a-tester.png" alt="QR code for this presentation" width="30%">
</img>

<sub>[LearnByFailure.com](https://learnbyfailure.com/thinking-like-a-tester/)</sub>

[comment]: # (!!!)

#### What is software testing?

----------

* Systematic way to verify whether or not software is doing what is intended
* [Majority of effort spent on software projects](https://dl.acm.org/doi/10.1145/3057269)
* Sooner we discover faults, the better
  * Automated tests
  * E.g. Unit tests - verify smallest "unit" of code
  * Regression tests


[comment]: # (!!!)

#### Example Unit Test

----------

* Given a function **place_piece** 
  * receives **row** and **column** coordinates (each 0-2, inclusive)
  * update the Tic-Tac-Toe board (3x3) of the current player's mark (`X` or `O`) if the space is not yet occupied and return the character of the piece placed
  * if there's already a piece there, return **'@'** 
  * or if the coordinates are invalid, return **'?'**.

[comment]: # (||| data-auto-animate)

#### Example Unit Test

----------

Example unit test assertions, 

comparing **actual** and **expected** values:

```ASSERT_EQ( game.place_piece(0,0) , 'X' );```

```ASSERT_EQ( game.place_piece(-1,0) , '?' );```


[comment]: # (!!!)

#### Conventional Testing Metrics

----------

* **Coverage** - After all tests are run, what % of production code has been run?
* **Testing effectiveness** - Percentage of faults caught by tests?
  * **Mutation Testing** - Make "mutant" variants of code, are they "killed" by tests?

* What are limitations to these metrics?

[comment]: # (!!!)

#### What about Positive Verification?

----------

* How reliably do tests **positively verify** software requirements?
* All-Function Pairs testing
  1. Classify each student's code (against instructor's tests) as **acceptable** or **buggy**
  2. Run each student's tests against each 
     {acceptable, buggy} implementation

[comment]: # (|||)

![](positive-verification.svg)

[comment]: # (|||)

* Found struggles with either **positive verification** *or* **test effectiveness**, but rarely both on the same function 
* Either type of inaccuracy is *equally associated* with number of bugs in *their own* code [(Buffardi \& Valdivia, 2019)](http://hdl.handle.net/10125/60199)

[comment]: # (!!!)

#### Measuring Testing Accuracy

----------

* How are **testing effectiveness** and **positive verification** related with a lack of faults? [(Buffardi, Valdivia, \& Rogers, 2019)](https://doi.org/10.1145/3287324.3287351)
  * **Accuracy** = (True Positives + True Negatives) / 
    (True Positives + True Negatives + False Positives + False Negatives)
  * **Accuracy** stronger correlation with lack of bugs than either **Test Effectiveness** or **Coverage**

[comment]: # (|||)

![Effectiveness vs Lack of Bugs, showing weak positive correlation](bug-finding.png)

(ρ=.355, p<.0167)

[comment]: # (|||)

![Coverage vs Lack of Bugs, showing moderate positive correlation](coverage.png)

(ρ=.514, p<.001)

[comment]: # (|||)

![Accuracy vs Lack of Bugs, showing strong positive correlation](accuracy.png)

(ρ=.648, p<.001)

[comment]: # (!!!)

##### How to Improve My Unit Tests?

----------

* Avoiding "Unit Test Smells" associated with **worse accuracy** [(Buffardi \& Aguirre-Ayala, 2021)](https://dl.acm.org/doi/abs/10.1145/3430665.3456328):
  * Each unit test case should only call one member function (when possible)
  * Keep unit tests simple without conditional logic (e.g. if's, loops, etc.)

[comment]: # (!!!)

##### How to THINK like a tester?

----------

  * Tversky and Kahneman’s [*Nobel Prize in Economics-winning research*](https://www.nobelprize.org/prizes/economic-sciences/2002/kahneman/facts/): consumers do not make decisions in their rational best interests because of cognitive biases
  * [**Cognitive Reflection Test**](https://www.aeaweb.org/articles?id=10.1257/089533005775196732) (CRT) tests the proficiency at inhibiting (fast) *"gut reaction"* in favor of (slower) *deliberate critical thinking*, e.g.:

A bat and a ball cost `$1.10` in total. The bat costs `$1.00` more than the ball. How much does the ball cost?

[comment]: # (|||)

* <small>Receive a string and return whether or not it is strictly a palindrome, where it is spelled the same backwards and forwards when considering every character in the string, but disregarding case ('x' is same as 'X')</small>

```
bool isPalindrome(string input){
  for( int i=0; i < input.size(); i++ ){
    if( input[i] < 'A' || input[i] > 'Z' ){
      input[i] = input[i] - ('a' - 'A');
    }
  }
  for( int i=0; i < input.size()/2; i++ ){
    if( input[i] != input[input.size()-1-i] )
      return false;
  }
  return true;
}
```

[comment]: # (|||)

* <small>Receive three integers and rearrange their values so that they are in descending order from greatest (first) to least (third)</small>

```
void sortDescending( int &first, int &second, int &third){
  if( first < second ){
    int temp = first;
    first = second;
    second = temp;
  }
  if( second < third ){
    int temp = second;
    second = third;
    third = temp;
  }
  if( first < third ){
    int temp = first;
    first = third;
    third = temp;
  }
}
```

[comment]: # (!!!)

#### CRT vs Code Review

----------

* CRT was a significant predictor of:
  * manually **rejecting defective code** (p<0.0001) with the log odds of correctly rejecting the defective code increasing by 2.94 (95% CI 1.56-4.50)
  * manually **identifying a defective case** (p<0.001) with the log odds of doing so increasing by 2.37 (95% CI 1.05-3.86).
* Reproduced association (ρ=0.478, p<0.01) in a study with a variant of CRT and different functions

[comment]: # (|||)

#### CRT vs Unit Testing

----------

* **BUT** no correlation between CRT and unit test accuracy in neither original (ρ=0.008, p=0.940) nor reproduction (ρ=0.113, p=0.498) studies [(Buffardi 2023)](https://doi.org/10.1109/ICSE-SEET58685.2023.00006)
* *Why do you think cognitive reflection is associated with manually reviewing code, but not unit test accuracy?*

[comment]: # (!!!)

#### Thinking Like a Tester

----------

<small>This presentation is accessible at [learnbyfailure.com/thinking-like-a-tester/](https://learnbyfailure.com/thinking-like-a-tester/) and its source is available on [GitHub](https://github.com/kbuffardi/thinking-like-a-tester/).</small>

<small>Special thanks to student co-authors: [Pedro Valdivia](https://www.linkedin.com/in/pedro-valdivia1/), [Destiny Rogers](https://www.linkedin.com/in/destiny-rogers/), and [Juan Aguirre-Ayala](https://www.linkedin.com/in/jaguirre-ayala/) who were all undergraduate researchers at Chico State.</small>

<img src="qr-code-thinking-like-a-tester.png" alt="QR code" width="30%">
</img>

<small>[Back to LearnByFailure](https://learnbyfailure.com/research/)
</small>
