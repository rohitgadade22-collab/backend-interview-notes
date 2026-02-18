C# Reference vs Value Types (Core Concept)
Simple Explanation

A variable of a class type does not store the object itself. It stores a reference (a pointer) to the object. The object holds the actual data.

Example
    class Box
    {
        public int Value;
    }
    var a = new Box();
    a.Value = 10;
    var b = a;
    b.Value = 20;
    Console.WriteLine(a.Value); // Output: 20
Reason: a and b both refer to the same object.

Method Parameter Behavior
static void Change(Box x)
{
    x.Value = 50;
}

var a = new Box { Value = 10 };
Change(a);
Console.WriteLine(a.Value); // 50

Object changed because method received copy of reference pointing to same object.

Reassignment Trap
static void Change(Box x)
{
    x = new Box();
    x.Value = 99;
}

var a = new Box { Value = 10 };
Change(a);
Console.WriteLine(a.Value); // 10

Reassignment changes only local reference, not original object.

Interview Questions

Q1: Difference between value type and reference type? Value types store actual data, reference types store reference to object containing data.

Q2: Why does modifying object inside method affect original? Because method receives a copy of the reference pointing to same object.

Q3: Why reassignment inside method does not affect caller? Because only local copy of reference changes.

Q4: How to allow reassignment to affect caller? Use ref or out keyword.

Q5: Where are objects stored? Objects in heap, variables in stack (conceptually for understanding).
