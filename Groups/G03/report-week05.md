# JOELLE

#Correction des tests sur le kata
J'ai effectué un test pour vérifier le mouvement d'un pion doit pouvoir avancer de deux case si premier mouvement.

``` pharo

testTargetSquaresLegal
	| pawn board targetSquares |
	board:= MyChessBoard empty.
	pawn := MyPawn new.
	pawn color: Color white.
	board at:'e2' put: pawn.
	targetSquares := pawn 
	targetSquaresLegal: true.
	
	"Vérification si avancement de deux cases un mouvement non legal"
	self assert: ((pawn targetSquaresLegal: true) first name) equals: 'e4'.

```

Le test est Jaune. L'assertion échoue parce qu'actuellement le pion ne peut avancer que d'une case à la fois
Par TDD, je vais vérifier si le mouvement demandé est le premier que le pion va effectuer pour implementer cette méthode.

---

# Bethuel Lafalaise

## Introduction to Design Patterns
Design patterns are elegant, generic, and proven solutions to recurring design problems in software development. 
They provide a vocabulary for discussing and organizing object-oriented designs, inspired by architectural patterns introduced by Christopher Alexander. 
While design patterns are not panaceas, they can greatly enhance modularity and reusability if applied judiciously. Patterns are categorized into creational, structural, and behavioral types, each with unique roles in software design.

## Message Sends Are Plans for Reuse
In object-oriented programming, message sends act as hooks for subclass behavior. 
They create opportunities for subclass-specific overrides, allowing customization without duplicating logic. Small methods are key to this philosophy, offering clear, encapsulated extension points.

## Hooks and Template
The template method is a powerful design pattern where the skeleton of an algorithm is defined in a superclass, with "hooks" for subclass-specific customizations. Hooks may have default implementations or require subclass overrides. 
This ensures consistency across implementations while allowing flexibility.
