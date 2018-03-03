---
layout: post
title:  "Google Hash Code"
date:   2018-03-03 22:00:00 +0100
tags: [code]
comments: true
---

# Hashcode 2018 by Google - Test Round

It's amazing the overwhelming number of code challenges on the web. Every other day I heard about a new one.
Last week I heard about [Hashcode](https://hashcode.withgoogle.com/#/) from Google, which is competion for teams around Europe, Middle East and Africa.
It has a qualification round, where your team can do remotely and the final is at Google HQ in Dublin.
<!--more-->

Once I mentioned to the challenge to Renata (my girlfriend) and 2 working colleagues, all of them liked the idea and we formed our team!
We were just missing a name, which was quickily solve by my girlfriend when she recommended **lasanha** !! Which as later changed to **lasagna** because my friends are Italians and finally to **lasagnas**.

We formed the team on Tuesday and the the qualification round would be on Thursday (2018-03-01), we didn't have much time to train/study.
On Wednesdy night we started doing the test statement, and on Thursday Salvo, Paolo and I stayed until late at work and did the test from there. Renata didn't participate on this fase :(.

In this post I will try to explain the problem and our solution to the test round. It's not an ideal neither elegant solution, but it got the job done with a fairly good score.

## Scores

Final Score: **47,953,286**
Score per file:

| File       | Score  |
| ---------- | ------ |
| example.in | 12     |
| small.in   | 35     |
| medium.in  | 49216  |
| big.in     | 894448 |

## Code

All the code is in the same repo on Github, just separated by branches. You can find some testing code there in other branches.

### Test Round

First, it's better to read understand the [statement](https://github.com/diegoluiz/google-hash-code-2018/blob/test-round/pizza.pdf).
The problem is simple to understand, although the solution is [it's nearly impossible to solve it completely](https://cs.stackexchange.com/questions/48775/np-hardness-of-covering-with-rectangular-pieces-google-hash-code-2015-test-roun).

#### Problem

Given a retangular pizza _HxW_ slices, slice it where the each small slice has the minimum _N_ of each of the 2 ingredients and still has less than _M_ in total.

For istance, given the following pizza, with the ingredients T and M, slice it perfectly where each slice has at least 1 of each ingredient and maximum of 6 integrientes in total.

```
TTTTT
TMMMT
TTTTT
```

The solution for this would be:

```
TT|T|TT
TM|M|MT
TT|T|TT
```

* The left slice has 6 ingredients and at least one of each
* The centre slice has 6 ingredients and at least one of each
* The right slice has 6 ingredients and at least one of each

It means, it's a valid solution, using 15 out of 15 ingredients, the maximum score.

## Our solution

Even though it's easy to find the solution for the example above, it gets complicate to find it in the code for a bigger set.
The solution we thought is quite simple, brute force each type of slice in the pizza.
Starting from one corner, tries to fit each valid shape of slice and move to the next space. Doing this consecutively until the pizza ends.
You can find the solution [here](https://github.com/diegoluiz/google-hash-code-2018/tree/test-round).

First we would need to build all types of valid shapes for the slices. This is related to the maximum number of ingredients and number of ingredients (2 for this exercise).
E.g. if the maximum number of ingredients is 4 and 2 ingredients. This means that we should have all shapes between 2 and 4 pieces:

```text
┌─┬─┐
└─┴─┘
┌─┬─┬─┐
└─┴─┴─┘
┌─┬─┬─┬─┐
└─┴─┴─┴─┘
┌─┬─┐
├─┼─┤
└─┴─┘
```

The piece of code to generate this is:

```csharp
private static List<SliceType> GetSliceTypes()
{
    var sliceTypes = new List<SliceType>();
    for (var rows = 1; rows <= Context.maxItems; rows++)
    {
        for (var cols = 1; cols <= Context.maxItems; cols++)
        {
            if (rows * cols > 1 && rows * cols <= Context.maxItems)
            {
                sliceTypes.Add(new SliceType { RowsCount = rows, ColsCount = cols });
            }
        }
    }

    sliceTypes = sliceTypes.OrderByDescending(x => x.ColsCount * x.RowsCount).ToList();
    return sliceTypes;
}
```

This will test all possible combinations for:

* all possible numbers up to the max ingredients. `rows = 1; rows <= Context.maxItems; rows++` and `cols = 1; cols <= Context.maxItems; cols++`, as no item will be bigger than the max.
* more than one piece per slice. `ows * cols > 1`, as we need space to fit at least 2 ingredients.
* and number of pieces is smaller than the max ingredients. `rows * cols <= Context.maxItems`.

Once all slice types are generated, we iterator throughout all pieces in the pizza

```csharp
for (var row = 0; row < pizza.RowsCount; row++)
{
    for (var col = 0; col < pizza.ColsCount; col++)
    {
        ...
    }
}
```

and foreach piece, it tries to fit any of the slice types.

```csharp
for (var row = 0; row < pizza.RowsCount; row++)
{
    for (var col = 0; col < pizza.ColsCount; col++)
    {
        foreach (var sliceType in sliceTypes)
        {
            if (row + sliceType.RowsCount > pizza.RowsCount || col + sliceType.ColsCount > pizza.ColsCount)
                continue;

            var row1 = row;
            var row2 = row + sliceType.RowsCount;
            var col1 = col;
            var col2 = col + sliceType.ColsCount;
            var components = new List<char>();
            for (var c = row; c < row2; c++)
            {
                for (var d = col; d < col2; d++)
                {
                    components.Add(pizza.Grid[c][d]);
                }
            }

            var slice = new Slice
            {
                Row1 = row1,
                Row2 = row2 - 1,
                Col1 = col1,
                Col2 = col2 - 1,
                Components = components
            };

            if (!slice.IsValidSlice())
                continue;

            if (sliceMap.IsSliceOverlapping(slice))
                continue;

            var sliceIndex = pizza.Slices.Count;
            pizza.Slices.Add(slice);
            sliceMap.Write(slice, sliceIndex);
        }
    }
}
```

A slice is fitted if:

* is not outside of the pizza borders;
* contains all componens;
* doesn't overlap another slice.

If the slice is valid, it is added to the pizza.

To validate the componens, when a slice is created based on the slice type, all components within it are added to the slice, and the method `IsValidSlice()` returns if the components within the slice match the requirements.

For the overlapping logic, Salvatore implemented a map (`SliceMap`),  storing the state of each slice, either in use or not. When a new valid slice is created and added to the pizza, the used pieces are added to the map, and it will have the state for the next iteration.

About the SliceMap. As it will reflect the pizza size, it's initialised using the number of rows and columns of the original pizza.
So in its constructor it receives the cols and rows from the pizza and initialises an array (`map`) with `rows` items and each item, `columns` items.
Each one containing the value `-1` which means that the piece is free..

```csharp
public SliceMap(int rows, int columns) {
    map = new int[rows][];
    for (int i = 0; i < rows; ++i) {
        map[i] = new int[columns];
        Array.Fill(map[i], -1);
    }
}
```

And to validate if the slice is overlapping another slice, method `IsSliceOverlapping`, it iterates from the source row/col to the target row/col, if any item is different than `-1` it means that the slice is overlapping.

```csharp
public bool IsSliceOverlapping(int row1, int col1, int row2, int col2) {
    for (var y = row1; y <= row2; ++y) {
        for (var x = col1; x <= col2; ++x) {
            if (map[y][x] != -1) {
                return true;
            }
        }
    }
    return false;
}
```

Once a slice is valid and not overlapping, its pieces are written into the `map` within the method `Write`.
It also iterates from the source row/col to the target row/col saving the id of the slice.

```csharp
for (var y = row1; y <= row2; ++y) {
    for (var x = col1; x <= col2; ++x) {
        map[y][x] = sliceIndex;
    }
}
```

I tried to make the problem and solution more descriptive as possible, so people who are programmers could understand how and why we did the challenge.

Soon I will try to post the code for the qualification round.