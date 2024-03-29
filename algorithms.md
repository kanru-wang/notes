## Analysis of Algorithms

- From https://courses.cs.washington.edu/courses/cse373/20su/lectures/5.pdf
    - (c): What is the constant factor?
    - (n0): From what point onward is f(n) smaller?
    - Case analysis -- Need Case Analysis when runtime is affected by input properties besides n.
    - Big Theta only exists when Big-O == Big-Omega
    - Big-O and Big-Omega do not have to be tight, but in industry, people often use Big-Oh to mean “Tight Big-Oh” and use it even when a Big-Theta exists

- From https://courses.cs.washington.edu/courses/cse373/20su/lectures/6.pdf
    - Master Theorem

- From https://brilliant.org/wiki/master-theorem/
    - Master Theorem

When calculating space complexity, input and output objects' space should not be taken into consideration.

When calculating time complexity, constant multiplier/coefficient should be ignored. When the time is the sum of multiple components, any component that is guaranteed to be smaller than the largest component should be ignored. 
