# Chord

Ask node n to find the successor of id
 
```
n.find_successor(id):
    if id ∈ (n, successor]
        return successor
    else
        n' = closest_preceding_node(id)
        return n'.find_successor(id)

```

Search the local table for the highest predecessor of id

```
n.closest_preceding_node(id):
    for i = m downto 1
        if finger[i] ∈ (n, id)
            return finger[i]
    return n
```

Create a new Chord ring

```
n.create():
    predecessor = nil
    successor = n
```

Join a Chord ring containing node n'

```
n.join(n'):
    predecessor = nil
    successor = n'.find_successor(n)
```

Called periodically. verifies n's immediate successor, and tells the successor about n

```
n.stabilize():
    x = successor.predecessor
    if x ∈ (n, successor)
        successor = x
    successor.notify(n)
```

n' thinks it might be our predecessor

```
n.notify(n'):
    if predecessor = nil or n' ∈ (predecessor, n)
        predecessor = n'
```

Called periodically. refreshes finger table entries.
Next stores the index of the finger to fix

```
n.fix_fingers():
    next = next + 1
    if next > m
        next = 1
    finger[next] = find_successor(n + 2^(next-1))
```

Called periodically. checks whether predecessor has failed

```
n.check_predecessor():
    if predecessor has failed
        predecessor = nil
```