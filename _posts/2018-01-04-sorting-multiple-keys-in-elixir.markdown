---
layout: post
title:  "TIL: Sorting by Multiple Keys in Elixir"
author: Robert Brown
date:   2018-01-04 21:30:00 -0600
categories:
---
While working on [Advent of Code 2017](http://adventofcode.com) I inadvertedly discovered how to sort collections by multiple keys. In the past, I've always written elaborate functions like this:

```
contacts
|> Enum.sort(fn x, y ->
  if x.last_name == y.last_name do
    x.first_name < y.first_name
  else
    x.last_name < y.last_name
  end
end)
```

Now I've found a simpler solution.

In the example above we have a list of contacts to sort by last name then first name. We can do so like this:

```
contacts
|> Enum.sort_by(&{&1.last_name, &1.first_name})
```

That's it. Just return a tuple with the list of values to sort by. (Note, this example uses `sort_by` instead of `sort`.) If two contacts have the same last name, then the first names will be compared. This can be done for any number of values.

This works with `min` and `max` too.

The down side of this approach is that it assumes that you want an ascending or descending sort for all the listed values. In general, you must use an elaborate function to handle mixed sort orders.

There is a special case for numbers though. Let's say you have a list of games and you want the game with the highest score. However, if two or more games have the same highest score, you want the first game to achieve that score. You can do so like this:

```
games
|> Stream.with_index
|> Enum.max_by(fn {x, n} -> {x.score, -n} end)
|> elem(0)
```

Basically, the index is multiplied by -1 to get the minimum.

Sorting is a common operation and it doesn't need to be complicated.
