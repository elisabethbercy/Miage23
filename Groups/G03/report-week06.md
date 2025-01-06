# Bethuel Lafalaise

## Understanding Double Dispatch
Double dispatch allows method execution based on the runtime types of two objects (the receiver and the argument). It eliminates the need for explicit conditionals (e.g., if, case) for decision-making.
Example: Rock-Paper-Scissors Game

In the game, the outcome of a match depends on both the receiver (e.g., Stone) and the argument (e.g., Paper).

```
Object subclass: #SPSElement.

SPSElement subclass: #Stone.
SPSElement subclass: #Paper.
SPSElement subclass: #Scissors.

"Stone"
Stone >> vs: anElement
    ^ anElement playAgainstStone.

Stone >> playAgainstStone
    ^ #draw.

Stone >> playAgainstPaper
    ^ #paper.

Stone >> playAgainstScissors
    ^ #stone.

"Paper"
Paper >> vs: anElement
    ^ anElement playAgainstPaper.

Paper >> playAgainstStone
    ^ #paper.

Paper >> playAgainstScissors
    ^ #scissors.

Paper >> playAgainstPaper
    ^ #draw.

"Scissors"
Scissors >> vs: anElement
    ^ anElement playAgainstScissors.

Scissors >> playAgainstPaper
    ^ #scissors.

Scissors >> playAgainstStone
    ^ #stone.

Scissors >> playAgainstScissors
    ^ #draw.

"Tests"
Stone new vs: Paper new.      "Output: #paper"
Paper new vs: Stone new.      "Output: #paper"
Scissors new vs: Paper new.   "Output: #scissors"
```

## Extensibility
The design supports adding new elements, like Spock and Lizard, without modifying existing classes.

```
SPSElement subclass: #Lizard.
SPSElement subclass: #Spock.

"New rules for Spock"
Spock >> vs: anElement
    ^ anElement playAgainstSpock.

Spock >> playAgainstStone
    ^ #spock.

Spock >> playAgainstPaper
    ^ #paper.

Spock >> playAgainstScissors
    ^ #spock.

Spock >> playAgainstLizard
    ^ #lizard.

Spock >> playAgainstSpock
    ^ #draw.
```

## Arithmetic with Double Dispatch
Double dispatch can extend arithmetic operations to support classes like Fraction.

```
Object subclass: #Number.
Number subclass: #Integer.
Number subclass: #Float.
Number subclass: #Fraction.

"Integer"
Integer >> + aNumber
    ^ aNumber sumWithInteger: self.

Integer >> sumWithInteger: anInteger
    ^ self + anInteger. "primitive addition"

Integer >> sumWithFloat: aFloat
    ^ self asFloat + aFloat.

Integer >> sumWithFraction: aFraction
    ^ Fraction numerator: (self * aFraction denominator) + aFraction numerator
              denominator: aFraction denominator.

"Fraction"
Fraction >> + aNumber
    ^ aNumber sumWithFraction: self.

Fraction >> sumWithInteger: anInteger
    ^ Fraction numerator: (self numerator + (anInteger * self denominator))
              denominator: self denominator.

Fraction >> sumWithFloat: aFloat
    ^ self asFloat + aFloat.

Fraction >> sumWithFraction: aFraction
    ^ Fraction numerator: (self numerator * aFraction denominator) + (aFraction numerator * self denominator)
              denominator: (self denominator * aFraction denominator).

"Usage"
(1 asInteger) + (3 asInteger).         "Output: 4"
(1/2) + (3/4).                        "Output: 5/4"
(1/2) + (2 asInteger).                "Output: 5/2"
```

